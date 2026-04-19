# Related Messages — picoCTF 2026
Hints given:
1. How are the two messages related?
2. Franklin Reiter _______ _______ attack.

Also in the description:

3. I have a typo in my first message so i sent it again! I used RSA twice so this is secure right?

The output.txt gives us `c1`, `c2`, `m2 = m1 + 3` and `N`. Also the chall.py gives us `e = 0x11 = 17`.

Since both messages are related and using the same `N` and `e` (and no padding) we can use the Franklin Reiter Related Message attack. 
The two polynomials `f1(x) = x^e - c1` and `f2(x) = (x + 3)^e - c2` share a common root `x = m1`. 
The corrected message can then be calculated by `m2 = m1 + 3` and converted back to bytes.

This has been done in the following python script:

```python
from math import comb
from Crypto.Util.number import long_to_bytes

# ---------- iterative extended GCD and modular inverse ----------

def egcd(a, b):
    x0, y0, x1, y1 = 1, 0, 0, 1
    while b != 0:
        q = a // b
        a, b = b, a % b
        x0, x1 = x1, x0 - q * x1
        y0, y1 = y1, y0 - q * y1
    return a, x0, y0

def modinv(a, n):
    g, x, _ = egcd(a % n, n)
    if g != 1:
        raise ValueError("No modular inverse")
    return x % n

# ---------- polynomial arithmetic over Z_N[x] ----------

def strip(p):
    while len(p) > 1 and p[-1] == 0:
        p.pop()
    return p

def poly_add(a, b, N):
    r = [0] * max(len(a), len(b))
    for i in range(len(a)):
        r[i] = (r[i] + a[i]) % N
    for i in range(len(b)):
        r[i] = (r[i] + b[i]) % N
    return strip(r)

def poly_sub(a, b, N):
    r = [0] * max(len(a), len(b))
    for i in range(len(a)):
        r[i] = (r[i] + a[i]) % N
    for i in range(len(b)):
        r[i] = (r[i] - b[i]) % N
    return strip(r)

def poly_mul(a, b, N):
    r = [0] * (len(a) + len(b) - 1)
    for i in range(len(a)):
        for j in range(len(b)):
            r[i + j] = (r[i + j] + a[i] * b[j]) % N
    return strip(r)

def poly_scalar(a, s, N):
    return [(x * s) % N for x in a]

def poly_divmod(a, b, N):
    a = a[:]
    b = strip(b[:])
    if len(b) == 0 or (len(b) == 1 and b[0] == 0):
        raise ZeroDivisionError("polynomial division by zero")

    inv_lead = modinv(b[-1], N)
    q = [0] * (len(a) - len(b) + 1)
    r = a[:]

    while len(r) >= len(b):
        factor = (r[-1] * inv_lead) % N
        deg = len(r) - len(b)
        q[deg] = factor
        for i in range(len(b)):
            r[deg + i] = (r[deg + i] - factor * b[i]) % N
        r = strip(r)

    return strip(q), strip(r)

def poly_gcd(a, b, N):
    a = strip(a)
    b = strip(b)
    while not (len(b) == 1 and b[0] == 0):
        _, r = poly_divmod(a, b, N)
        a, b = b, r
    inv = modinv(a[-1], N)
    return poly_scalar(a, inv, N)

# ---------- Franklin–Reiter for m2 = m1 + k ----------

def franklin_reiter(N, e, c1, c2, k):
    # f1(x) = x^e - c1
    f1 = [0] * (e + 1)
    f1[0] = (-c1) % N
    f1[e] = 1

    # f2(x) = (x + k)^e - c2
    f2 = [0] * (e + 1)
    for i in range(e + 1):
        f2[i] = (comb(e, i) * pow(k, e - i, N)) % N
    f2[0] = (f2[0] - c2) % N

    g = poly_gcd(f1, f2, N)
    if len(g) != 2:
        raise ValueError("GCD is not linear, attack failed")

    # g(x) = x - m1  (monic), so root is m1
    m1 = (-g[0]) % N
    m2 = (m1 + k) % N
    return m1, m2

# ---------- challenge values ----------

N  = 17334845546772507565250479697360218105827285681719530148909779921509619103084219698006014339278818598859177686131922807448182102049966121282308256054696565796008642900453901629937223685292142986689576464581496406676552201407729209985216274086331582917892470955265888718120511814944341755263650688063926284195007148056359887333784052944201212155189546062807573959105963160320187551755272391293705288576724811668369745107148481856135696249862795476376097454818009481550162364943945249601744881676746859305855091288055082626399929893610275614840617858985993338556889612804266896309310999363054134373435198031731045253881
c1 = 3486364849772584627692611749053367200656673358261596068549224442954489368512244047032432842601611650021333218776410522726164792063436874469202000304563253268152374424792827960027328885841727753251809392141585739745846369791063025294100126955644910200403110681150821499366083662061254649865214441429600114378725559898580136692467180690994656443588872905046189428367989340123522629103558929469463071363053880181844717260809141934586548192492448820075030490705363082025344843861901475648208157572346004443100461870519699021342998731173352225724445397168276113254405106732294978648428026500248591322675321980719576323749
c2 = 201982790559548563915678784397933493721879152787419243871599124287434576744055997870874349538398878336345269929647585648144070475012256331468688792105087899416655051702630953882466457932737483198442642588375981620937494661378586614008496182135571457352400128892078765628319466855732569272509655562943410536265866312968101366413636251672211633011159836642751480632253423529271185888171036917413867011031963618529122680143291205470937752671602494831117301480813590683791618751348224964277861127486155552153012612562009905595646626759034581358425916638671884927506025703373056113307665093346439014722219878575598308124
e  = 17
k  = 3  # Message_fixed = Message + 3

if __name__ == "__main__":
    m1, m2 = franklin_reiter(N, e, c1, c2, k)
    print("m1 =", m1)
    print("m2 =", m2)

    print("check c1:", pow(m1, e, N) == c1)
    print("check c2:", pow(m2, e, N) == c2)
    print("check relation:", (m1 + k) % N == m2)

    print("m2 bytes:", long_to_bytes(m2))
```
