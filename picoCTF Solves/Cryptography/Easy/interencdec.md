# **picoCTF Challenge:** ***interencdec***

- [Challenge information](#challenge-information)
- [Solution](#solution)
- [References](#references)

---

## **Challenge information**

<table>
  <tr><td><strong>Level</strong></td><td>🟢 Easy</td></tr>
  <tr><td><strong>Category</strong></td><td>Cryptography</td></tr>
  <tr><td><strong>Event</strong></td><td>picoCTF 2024</td></tr>
  <tr><td><strong>Author</strong></td><td>NGIRIMANA Schadrack</td></tr>
</table>

---

**Description:**
Can you get the real meaning from this file.
Download the file here.
> I do not wish to link the download, but [here](https://play.picoctf.org/practice/challenge/418) is the challenge post on picoCTF

---

> **Hint:** Engaging in various decoding processes is of utmost importance

---


## **Solution**

### View
First we must read the file to determine what amalgamation of horror Ngirimana has created.
```bash
Ridez-picoctf@webshell:~$ cat enc_flag
YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclgya3lNRFJvYTJvMmZRPT0nCg==
```

---

### Decode
A simple base64 sting encoding.
```bash
Ridez-picoctf@webshell:~$ echo "YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclgya3lNRFJvYTJvMmZRPT0nCg==" | base64 -d
```
Returns:
```text
b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX2kyMDRoa2o2fQ=='
```
In this text, we can see the single quote around the string. That informs me that the b and quotes are extra characters.

---

### Decode Part 2
Removing the extra characters and running a base64 decode again returns a sequence of characters that look like a typical flag.
```bash
Ridez-picoctf@webshell:~$ echo "d3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX2kyMDRoa2o2fQ=
=" | base64 -d
```
Returns:
```text
wpjvJAM{jhlzhy_k3jy9wa3k_i204hkj6}
```

---

### Why Ceasar?
Within the flag are underscores. In ceasar ciphers, special characters like curly brackets and underscores are ignored. This is a classic ceasar shift. Placing it into [Cryptii Ceasar Cipher](https://cryptii.com/pipes/caesar-cipher/), I shift the characters until I see the flag.


After a 19 character shift, we have the flag.
```text
picoCTF{caesar_d3cr9pt3d_Redacted}
```


## **References**
[Cryptii Ceasar Cipher](https://cryptii.com/pipes/caesar-cipher/)