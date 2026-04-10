# **picoCTF Challenge:** ***Python Wrangling***

- [Challenge information](#challenge-information)
- [Solution](#solution)
- [Reference](#reference)

---

## **Challenge information**

<table>
  <tr><td><strong>Level</strong></td><td>🟡 Medium</td></tr>
  <tr><td><strong>Category</strong></td><td>General Skills</td></tr>
  <tr><td><strong>Event</strong></td><td>picoCTF 2021</td></tr>
  <tr><td><strong>Author</strong></td><td>syreal</td></tr>
</table>

---

**Description:**
Python scripts are invoked kind of like programs in the Terminal...
Can you run ende.py using password.txt to get flag.txt.en?
> I do not wish to link the downloads, but [here](https://play.picoctf.org/practice/challenge/166) is the challenge post on picoCTF
---

> **Hint:**
Get the Python script accessible in your shell by entering the following command in the Terminal prompt: $ wget followed by a link to the script. The link can be copied from the details section.

---

## **Solution**

### **Step 1 — Downloading the files**
First, we need to collect all the files so they're accessible.

```bash
Ridez-picoctf@webshell:~$ wget https://challenge-files.picoctf.net/c_wily_couri
er/d8ab9bfd6822fadbdfa9faffb487dab337afaf8c83d447a1b954373e15bc7d7e/ende.py | w
get https://challenge-files.picoctf.net/c_wily_courier/d8ab9bfd6822fadbdfa9faff
b487dab337afaf8c83d447a1b954373e15bc7d7e/password.txt | wget https://challenge-
files.picoctf.net/c_wily_courier/d8ab9bfd6822fadbdfa9faffb487dab337afaf8c83d447
a1b954373e15bc7d7e/flag.txt.en
```

---

> *A great deal of the return text is clutter and not important to solving the flag. I have only included enough to be visually appealing while neccessary.*

```bash
Length: 140 [application/octet-stream]
Saving to: 'flag.txt.en'

flag.txt.en         100%[==================>]     140  --.-KB/s    in 0s      

2026-04-10 10:42:20 (29.9 MB/s) - 'flag.txt.en' saved [140/140]

HTTP request sent, awaiting response... 200 OK
Length: 33 [application/octet-stream]
Saving to: 'password.txt'

password.txt        100%[==================>]      33  --.-KB/s    in 0s      

2026-04-10 10:42:20 (10.3 MB/s) - 'password.txt' saved [33/33]

200 OK
Length: 1328 (1.3K) [application/octet-stream]
Saving to: 'ende.py'

ende.py             100%[==================>]   1.30K  --.-KB/s    in 0s      

2026-04-10 10:42:20 (337 MB/s) - 'ende.py' saved [1328/1328]
```

---
### **Step 2 — Reading the Python script**
Now to view the main python file.
```bash
Ridez-picoctf@webshell:~$ cat ende.py
```

```py
import sys
import base64
from cryptography.fernet import Fernet

usage_msg = "Usage: "+ sys.argv[0] +" (-e/-d) [file]"
help_msg = usage_msg + "\n" +\
        "Examples:\n" +\
        "  To decrypt a file named 'pole.txt', do: " +\
        "'$ python "+ sys.argv[0] +" -d pole.txt'\n"

if len(sys.argv) < 2 or len(sys.argv) > 4:
    print(usage_msg)
    sys.exit(1)

if sys.argv[1] == "-e":
    if len(sys.argv) < 4:
        sim_sala_bim = input("Please enter the password:")
    else:
        sim_sala_bim = sys.argv[3]

    ssb_b64 = base64.b64encode(sim_sala_bim.encode())
    c = Fernet(ssb_b64)

    with open(sys.argv[2], "rb") as f:
        data = f.read()
        data_c = c.encrypt(data)
        sys.stdout.write(data_c.decode())

elif sys.argv[1] == "-d":
    if len(sys.argv) < 4:
        sim_sala_bim = input("Please enter the password:")
    else:
        sim_sala_bim = sys.argv[3]

    ssb_b64 = base64.b64encode(sim_sala_bim.encode())
    c = Fernet(ssb_b64)

    with open(sys.argv[2], "r") as f:
        data = f.read()
        data_c = c.decrypt(data.encode())
        sys.stdout.buffer.write(data_c)

elif sys.argv[1] == "-h" or sys.argv[1] == "--help":
    print(help_msg)
    sys.exit(1)

else:
    print("Unrecognized first argument: "+ sys.argv[1])
    print("Please use '-e', '-d', or '-h'.")
```

This script supports two arguments.
> `-e` for encryption and `-d` for decryption. 

To decrypt `flag.txt.en`, we need to use the -d flag along with the filename. When we use the `-d` flag, it will check for if there is less than four arguments. 

`ende.py` is arg[0]
`-d` is arg[1]
`pole.txt` is arg[2]

> Remember, argments begin indexing at 0.

---
### **Step 3 — Renaming the file and reading the password**
We know the correct name and can now dump the flag.
```bash
Ridez-picoctf@webshell:~$ cp flag.txt.en pole.txt                             
Ridez-picoctf@webshell:~$ cat password.txt
720b6ad346f84cd483c60c7464dd95d4
```
The script's built-in help message uses `pole.txt` as its example filename. We rename flag.txt.en to `pole.txt` to keep everything consistent. `cat password.txt` reveals the decryption key we'll need to pass in the next step.

---

### **Step 4 — Decrypting the flag**
```bash
Ridez-picoctf@webshell:~$ python3 ende.py -d pole.txt
Please enter the password:720b6ad346f84cd483c60c7464dd95d4
picoCTF{redacted}
```
The Fernet algorithm is a symmetric key encrytion algorithm. The same key that encrypted the file can decrypt it. The string from `password.txt` decrypted pole.txt and gave us the flag.


## **Reference**
- [Fernet symmetric encryption - cryptography library](https://cryptography.io/en/latest/fernet/)