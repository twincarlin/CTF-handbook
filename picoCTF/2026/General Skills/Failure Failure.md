# Failure Failure — picoCTF 2026

Download the challenge files:

    wget https://challenge-files.picoctf.net/c_mysterious_sea/39702958ec24b31bdf69f92245878336c8875d2542335f233fc94d7ec73e8be9/haproxy.cfg
    wget https://challenge-files.picoctf.net/c_mysterious_sea/39702958ec24b31bdf69f92245878336c8875d2542335f233fc94d7ec73e8be9/app.py

Inspect them:

    cat haproxy.cfg
    cat app.py

Visit the site:  
http://mysterious-sea.picoctf.net:61416/

The key logic in `app.py`:

    @limiter.limit("300 per minute")
    def home():
        print("value:", os.getenv("IS_BACKUP"))
        if os.getenv("IS_BACKUP") == "yes":
            flag = os.getenv("FLAG")

So:
- The main server normally does **not** show the flag.
- But if the rate‑limit is exceeded, HAProxy routes traffic to the **backup server**.
- The backup server has `IS_BACKUP="yes"` and therefore reveals the flag.

Trigger the failover by exceeding 300 requests/min:

    for i in {1..400}; do curl -s http://mysterious-sea.picoctf.net:61416/ > /dev/null; done

Now reload the page:  
http://mysterious-sea.picoctf.net:61416/

The backup server responds and displays the flag.
