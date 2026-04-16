# **picoCTF Challenge:** ***Mod26***

- [Challenge information](#challenge-information)
- [Solution](#solution)
- [References](#references)

---

## **Challenge information**

<table>
  <tr><td><strong>Level</strong></td><td>🟢 Easy</td></tr>
  <tr><td><strong>Category</strong></td><td>Cryptography</td></tr>
  <tr><td><strong>Event</strong></td><td>picoCTF 2021</td></tr>
  <tr><td><strong>Author</strong></td><td>Pandu</td></tr>
</table>

---

**Description:**
Cryptography can be easy, do you know what ROT13 is?
values.txt
> I do not wish to link the download, but [here](https://play.picoctf.org/practice/challenge/144) is the challenge post on picoCTF

---

> **Hint:** This can be solved online if you don't want to do it by hand!

---


## **Solution**
View the file.
```bash
Ridez-picoctf@webshell:~$ cat values.txt
cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_45559noq}
```

---

Placing the string into a ROT13 converter gives us the flag.
```text
picoCTF{next_time_I'll_try_2_rounds_of_rot13_Redacted}
```



## **References**
[CyberChef](https://gchq.github.io/CyberChef/)