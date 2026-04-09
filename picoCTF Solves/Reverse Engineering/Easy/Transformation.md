# **picoCTF Challenge:** ***Transformation***

- [Challenge information](#challenge-information)
- [Solution](#solution)
- [References](#references)

---

## **Challenge information**

<table>
  <tr><td><strong>Level</strong></td><td>🟢 Easy</td></tr>
  <tr><td><strong>Category</strong></td><td>Reverse Engineering</td></tr>
  <tr><td><strong>Event</strong></td><td>picoCTF 2021</td></tr>
  <tr><td><strong>Author</strong></td><td>madStacks</td></tr>
</table>

---

**Description:**
I wonder what this really is...
enc ''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)])
> I do not wish to link the download, but [here](https://play.picoctf.org/practice/challenge/104) is the challenge post on picoCTF

---

> **Hint:** You may find some decoders online

---


## **Solution**


## **References**