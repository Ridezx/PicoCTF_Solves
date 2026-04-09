# **picoCTF Challenge:** ***Undo***

- [Challenge information](#challenge-information)
- [Solution](#solution)
- [References](#references)

---

## ***Challenge information***

<table>
  <tr><td><strong>Level</strong></td><td>🟢 Easy</td></tr>
  <tr><td><strong>Category</strong></td><td>General Skills</td></tr>
  <tr><td><strong>Event</strong></td><td>picoCTF 2026</td></tr>
  <tr><td><strong>Author</strong></td><td>Yahaya Meddy</td></tr>
</table>

---

**Description:**
> Can you reverse a series of Linux text transformations to recover the original flag? Additional details will be available after launching your challenge instance.

---

**Connect via netcat:**
```
nc foggy-cliff.picoctf.net 55748
```

> **Hint:** For text translation and character replacement, see [tr command documentation](https://man7.org/linux/man-pages/man1/tr.1.html).

---

## ***Solution***

After connecting to `foggy-cliff.picoctf.net`, we are greeted with the challenge start screen.

---

### **Step 1 — Reverse Base64 encoding**

**Terminal output:**
```bash
(Ridez@webshell:~$) nc foggy-cliff.picoctf.net 55748
===Welcome to the Text Transformation Challenge!===

Your Goal: step by step, recover the original flag.
At each step, you will see the transformed flag and a hint.
Enter the correct Linux command to reverse the last transformation.

--- Step 1 ---
Current flag: KXA2OTgxcHBxLWZhMDFnQHplMHNmYTRlRy1nazNnLXRhMWZlcmlyRShTR1BicHZj
Hint: Base64 encoded the string.
Enter the Linux command to reverse it:
```
---

**Command:**

> Within picoCTF there is a webshell built into the website. This challenge only requires that we type the Linux command it wants. In this example, I only needed to type "base64 -d". In a real scenario, the command would look like:

```bash 
echo "KXA2OTgxcHBxLWZhMDFnQHplMHNmYTRlRy1nazNnLXRhMWZlcmlyRShTR1BicHZj" | base64 -d
```

Within picoCTF terminal, I type:

```bash
base64 -d
```
> If you were to type this into your own terminal, it would not decode anything. There is no argument passed for it to decode.

**Reasoning:**
Base64 is an encoding scheme that converts binary data into ASCII characters. The `-d` flag decodes the string back to its original form. It is not encryption since no key is needed, just the reverse operation.

---

### **Step 2 — Reverse text reversal**

**Terminal Output**
```bash
--- Step 2 ---
Current flag: )p6981ppq-fa01g@ze0sfa4eG-gk3g-ta1ferirE(SGPbpvc
Hint: Reversed the text.
Enter the Linux command to reverse it: 
```

**Command:**
```bash
rev
```

**Reasoning:**
The 'rev' command reverses the order of characters on a line. Since the transformation was a reversal, applying 'rev' again undoes it. Reversing a reversal returns you to the original string.

---

### **Step 3 — Restore dashes to underscores**

**Terminal Output**
```bash
--- Step 3 ---
Current flag: cvpbPGS(Eriref1at-g3kg-Ge4afs0ez@g10af-qpp1896p)
Hint: Replaced underscores with dashes.
Enter the Linux command to reverse it:
```

**Command:**
```bash
tr '-' '_'
```

**Reasoning:**
The 'tr' command translates characters one-for-one. Since the original transformation swapped '_' for '-', we reverse it by swapping '-' back to '`_`'.

---

### **Step 4 — Restore curly braces**

**Terminal Output**
```bash
--- Step 4 ---
Current flag: cvpbPGS(Eriref1at_g3kg_Ge4afs0ez@g10af_qpp1896p)
Hint: Replaced curly braces with parentheses.
Enter the Linux command to reverse it: 
```

**Command:**
```bash
tr '()' '{}'
```

**Reasoning:**
Again using 'tr', this time targeting parentheses '()' and replacing them with curly braces '{}', which is the standard bracket style used in all picoCTF flags.

---

### **Step 5 — Reverse ROT13**

**Terminal Output**
```bash
--- Step 5 ---
Current flag: cvpbPGS{Eriref1at_g3kg_Ge4afs0ez@g10af_qpp1896p}
Hint: Applied ROT13 to letters.
Enter the Linux command to reverse it: 
```

**Command:**
```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

**Reasoning:**
ROT13 shifts every letter 13 positions in the alphabet. Because there are 26 letters, applying ROT13 twice returns the original. It is its own inverse. The 'tr' command handles both uppercase and lowercase simultaneously.

---

## **Confirmation and Flag**
```bash
Congratulations! You've recovered the original flag:
>>> picoCTF{Redacted}
```
> *I am redacting the flag because I am well aware that as a public repository, this is also visible to my peers. I encourage those who are looking for writeups to follow along with the hints provided to you and attempt to get the flag youself instead of copying it from another.*

---

## ***References***

- [tr command documentation](https://man7.org/linux/man-pages/man1/tr.1.html)
- [Base64 - Wikipedia](https://en.wikipedia.org/wiki/Base64)
- [ROT13 - Wikipedia](https://en.wikipedia.org/wiki/ROT13)