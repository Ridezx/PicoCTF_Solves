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

### **Step 1 — Understanding the encoding**

```python
''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)])
```

This walks through the flag two characters at a time and:
- Takes the first character and bit-shifts it left by 8 (`<< 8`), equivalent to multiplying by 256
- Adds the second character's ASCII value to it
- Packs both values into a single Unicode character using `chr()`

The result is a string roughly half the length of the original flag, where each Unicode character silently contains two ASCII characters packed inside it. To reverse it, we need to unpack each Unicode character back into two ASCII characters by splitting the high and low bytes apart.

---

### **Step 2 — Downloading and opening the file**

```bash
wget https://challenge-files.picoctf.net/c_wily_courier/.../enc
```

```bash
Ridez-academy@webshell:~$ python
>>> enc = open("enc").read()
>>> print(enc)
灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸形㝦Redacted
```
Each of these characters is 2 of our flag. To confirm, we take the first character and decode it.

```bash
>>> print(enc[0])
灩
>>> print(hex(ord(enc[0])))
0x7069
```

**Reasoning:**
The file contains a string of Unicode characters. Checking the first character `灩` reveals its hex value is `0x7069`. Breaking that apart:
- `0x70` = 112 = `p`
- `0x69` = 105 = `i`

The first two characters of the flag (`pi`) are packed into that single character. This confirms the encoding works exactly as described. Seeing `pi` shows that we only need to decode the rest of the characters in the same way.

---

### **Step 3 — Decode each character**

```bash
>>> for i in enc:
...     print(hex(ord(i)).lstrip("0x"), end='')
...
7069636f4354467b31365f626974735f696e73743334645f6f665f385f6237Redacted
```

**Reasoning:**
This loop iterates over every Unicode character in `enc`, converts it to its hex value with `hex(ord(i))`, `lstrip` strips the `0x` prefix, and prints them all without spaces. We now have a single string.

---


### **Step 4 — Hex to ASCII**

The hex string is decoded using CyberChef's `From Hex` function. 

Input:
```
7069636f4354467b31365f626974735f696e73743334645f6f665f385f62Redacted
```

Output:
```
picoCTF{16_bits_inst34d_of_8_Redacted}
```

---

## **References**
- [CyberChef - From Hex](https://gchq.github.io/CyberChef/)
- [Python Ord Function](https://www.w3schools.com/python/ref_func_ord.asp)