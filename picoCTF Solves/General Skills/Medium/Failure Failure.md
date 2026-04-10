# **picoCTF Challenge:** ***Failure Failure***

- [Challenge information](#challenge-information)
- [Solution](#solution)
- [References](#references)

---

## **Challenge information**

<table>
  <tr><td><strong>Level</strong></td><td>🟡 Medium</td></tr>
  <tr><td><strong>Category</strong></td><td>General Skills</td></tr>
  <tr><td><strong>Event</strong></td><td>picoCTF 2026</td></tr>
  <tr><td><strong>Author</strong></td><td>Darkraicg492</td></tr>
</table>

---

**Description:**
Welcome to Failure Failure — a high-available system.
This challenge simulates a real-world failover scenario where one server is prioritized over the other.
A load balancer stands between you and the truth — and it won't hand over the flag until you force its hand.
You can begin your journey here to try and retrieve the flag.
For reference:
The HAProxy configuration used in this challenge is available here.
The application code is available here
> I do not wish to link the downloads, but [here](https://play.picoctf.org/practice/challenge/756) is the challenge post on picoCTF

---

> **Hint:** How does a load balancer decide which server should get the traffic?

---
### *Extra Note:* ###
Some of the few classes I took in person at Cuyahoga Community College were the Cisco courses offered. Within Cisco II, at least a quarter of the class was focused on active-passive and active-active load balancing setups. When I read the description, I knew I had to add it to the list of medium-level solves.

## **Solution**
Within a network, a load balancer is a device that distributes incoming traffic to multiple servers to reduce the potential stress on a single server. In this particular scenario, there are two servers. One is configured to be active, while the other is passive. 


First, we access the website hosted by picoCTF. It greets us with a simple message.

```text
Welcome!!
---
No flag in this service
```

---

We're given two files alongside the website. Now to download them.

---

```bash
Ridez-picoctf@webshell:~$ wget https://challenge-files.picoctf.net/c_mysterious_sea/8bb2088455f98bd780bc734703354b8ca9cdac17011b1a833a89a1781a4fec46/haproxy.cfg
--2026-04-10 08:10:33--  https://challenge-files.picoctf.net/c_mysterious_sea/8bb2088455f98bd780bc734703354b8ca9cdac17011b1a833a89a1781a4fec46/haproxy.cfg
Resolving challenge-files.picoctf.net (challenge-files.picoctf.net)... 3.160.5.64, 3.160.5.18, 3.160.5.95, ...
Connecting to challenge-files.picoctf.net (challenge-files.picoctf.net)|3.160.5.64|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 425 [application/octet-stream]
Saving to: 'haproxy.cfg'

haproxy.cfg         100%[==================>]     425  --.-KB/s    in 0s      

2026-04-10 08:10:33 (18.1 MB/s) - 'haproxy.cfg' saved [425/425]
```
```bash
Ridez-picoctf@webshell:~$ wget https://challenge-files.picoctf.net/c_mysterious_sea/8bb2088455f98bd780bc734703354b8ca9cdac17011b1a833a89a1781a4fec46/app.py
--2026-04-10 08:11:29--  https://challenge-files.picoctf.net/c_mysterious_sea/8bb2088455f98bd780bc734703354b8ca9cdac17011b1a833a89a1781a4fec46/app.py
Resolving challenge-files.picoctf.net (challenge-files.picoctf.net)... 3.160.5.64, 3.160.5.95, 3.160.5.40, ...
Connecting to challenge-files.picoctf.net (challenge-files.picoctf.net)|3.160.5.64|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 850 [application/octet-stream]
Saving to: 'app.py'

app.py              100%[==================>]     850  --.-KB/s    in 0s      

2026-04-10 08:11:29 (165 MB/s) - 'app.py' saved [850/850]
```

---

Now that they are on our system, we can view the juicy details that will allow us to snatch this flag.

---

```bash
Ridez-picoctf@webshell:~$ cat haproxy.cfg
# haproxy.cfg
global
    log stdout format raw local0
    maxconn 1000

defaults
    log global
    mode http
    timeout connect 5s
    timeout client 10s
    timeout server 10s
    
frontend http-in
    bind *:80
    default_backend servers

backend servers
    option httpchk GET /
    http-check expect status 200
    server s1 *:8000 check inter 2s fall 2 rise 3
    server s2 *:9000 check backup inter 2s fall 2 rise 3
```

---

There are two servers configured on the system. Server 1 is likely the active server that shows the webpage. If we somehow manage to take server 1 down, server two will take over and show us the flag. We now need to find a way to overload the server, but we don't know its parameters.

---

```bash
Ridez-picoctf@webshell:~$ cat app.py  
from flask import Flask, render_template
from dotenv import load_dotenv
from flask_limiter import Limiter
import os

load_dotenv()

app = Flask(__name__)

# Custom key function for global rate limiting
def global_rate_limit_key():
    return "global"

# Initialize rate limiter with global key function
limiter = Limiter(
    key_func=global_rate_limit_key,
    app=app,
    default_limits=["300 per minute"]
)

# Custom error handler for rate limit exceeded
@app.errorhandler(429)
def ratelimit_exceeded(e):
    return "Service Unavailable: Rate limit exceeded", 503

@app.route('/')
@limiter.limit("300 per minute")
def home():
    print("value:", os.getenv("IS_BACKUP"))
    if os.getenv("IS_BACKUP") == "yes":
        flag = os.getenv("FLAG")
    else:
        flag = "No flag in this service"
    return render_template("index.html", flag=flag)
```
Within the `limiter` variable, we see that the limit for requests is 300 per minute. If we ping the server more than 300 times in a minute, it will go down, giving us the flag.  

---

```bash
Ridez-picoctf@webshell:~$ for i in {1..301}; do curl -s http://mysterious-sea.picoctf.net:54455/ > /dev/null; done 
```
This is a Bash `for` loop that runs 301 times. The command will iterate from 1 to 301, running the body of the loop each time. `curl -s http://mysterious-sea.picoctf.net:54455/` sends an HTTP GET request to the primary server. `-s` flag runs `curl` silently. `> /dev/null` discards the server's response so it doesn't clutter the terminal. We don't care to get a bunch of messages to clutter our whole screen. 



A quick reload of the website gives us a new front page.
```text
Welcome!!
picoCTF{redacted}
```

## **References**
- [HAProxy Documentation](https://www.haproxy.org/#docs)
- [Active-Passive Load Balancing - Cloudflare](https://www.cloudflare.com/learning/performance/what-is-load-balancing/)