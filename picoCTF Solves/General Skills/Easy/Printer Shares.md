# **picoCTF Challenge:** ***Printer Shares***

- [Challenge information](#challenge-information)
- [Solution](#solution)
- [References](#references)

---

## Challenge information

<table>
  <tr><td><strong>Level</strong></td><td>🟢 Easy</td></tr>
  <tr><td><strong>Category</strong></td><td>General Skills</td></tr>
  <tr><td><strong>Event</strong></td><td>picoCTF 2026</td></tr>
  <tr><td><strong>Author</strong></td><td>Janice He</td></tr>
</table>

---

**Description:**
Oops! Someone accidentally sent an important file to a network printer—can you retrieve it from the print server?

---

> **Hint:** Knowing how SMB protocol works would be helpful! `smbclient` and `smbutil` are good tools.

---

## Solution

Server Message Block is a network protocol primarily used for sharing files, printers, and other resources between machines on a network. In this challenge, a file was accidentally sent to a networked printer's share. What this means is the file is sitting on an SMB server waiting to be retrieved. I need to connect to that server, locate the file, and download it.

---

### Step 1 — Listing available shares

```bash
Ridez-picoctf@webshell:~$ smbclient -L //mysterious-sea.picoctf.net -p 53700 -N

        Sharename       Type      Comment
        ---------       ----      -------
        shares          Disk      Public Share With Guests
        IPC$            IPC       IPC Service (Samba 4.19.5-Ubuntu)
SMB1 disabled -- no workgroup available
```

**Explaination:**
The '-L' flag tells smbclient to list all available shares on the target server without connecting to any of them. The '-p 53700' specifies the port, and '-N' suppresses the password prompt since this is a guest-accessible server.

---

### Step 2 — Connecting to the share

```bash
Ridez-picoctf@webshell:~$ smbclient //mysterious-sea.picoctf.net/shares -p 53700 -N
Try "help" to get a list of possible commands.
smb: \>
```

**Explaination:**
We connect directly to the 'shares' share this time by specifying it in the path. A successful connection drops us into a shell, indicated by the 'smb: `\>`' prompt. Now that we know 'shares' exists and is publicly accessible, we connect to it directly.

---

### Step 3 — Listing the contents

```bash
smb: \> ls
  .                                   D        0  Fri Mar  6 20:25:41 2026
  ..                                  D        0  Fri Mar  6 20:25:41 2026
  dummy.txt                           N     1142  Wed Feb  4 21:22:17 2026
  flag.txt                            N       37  Fri Mar  6 20:25:41 2026

                65536 blocks of size 1024. 58232 blocks available
smb: \>
```

**Explaination:**
The 'ls' command inside the SMB shell is no different to 'ls' in a Linux terminal. It lists the files and directories in the current location. Two files are present: `dummy.txt` and `flag.txt`. picoCTF solve codes are called flags. 'flag.txt'is only 37 bytes and named flag. It can't get more obvious than that.

---

### Step 4 — Retrieving and reading the flag

```bash
smb: \> get flag.txt
getting file \flag.txt of size 37 as flag.txt (9.0 KiloBytes/sec) (average 9.0 KiloBytes/sec)
smb: \> exit
Ridez-picoctf@webshell:~$ cat flag.txt
picoCTF{Redacted}
```

**Explaination:**
The 'get' command downloads 'flag.txt' from the SMB share to our local working directory. We then exit the SMB shell and use 'cat' to print the contents of the downloaded file to the terminal. The flag confirms the file was the one accidentally sent to the printer share, and the challenge is complete.

---

## References

- [smbclient - Hackviser](https://hackviser.com/tactics/tools/smbclient)
- [SMB Protocol - Wikipedia](https://en.wikipedia.org/wiki/Server_Message_Block)