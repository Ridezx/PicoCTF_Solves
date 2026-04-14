# **picoCTF Challenge:** ***Corrupted file***

- [Challenge information](#challenge-information)
- [Solution](#solution)
- [References](#references)

---

## **Challenge information**

<table>
  <tr><td><strong>Level</strong></td><td>🟢 Easy</td></tr>
  <tr><td><strong>Category</strong></td><td>Forensics</td></tr>
  <tr><td><strong>Event</strong></td><td>picoMini by CMU-Africa</td></tr>
  <tr><td><strong>Author</strong></td><td>Yahaya Meddy</td></tr>
</table>

---

**Description:**
This file seems broken... or is it? Maybe a couple of bytes could make all the difference. Can you figure out how to bring it back to life?
Download the file here.
> I do not wish to link the download, but [here](https://play.picoctf.org/practice/challenge/519) is the challenge post on picoCTF

---

> **Hint:** Try checking the file’s header. Jpeg. Tools like xxd or hexdump can help you inspect and edit file bytes.

---


## **Solution**
Within files, whether its a jpeg, png, pdf, or gif, they have a string of hexadecimal base-16 codes in the header of the file. Calling to the hint, we are pushed in the direction of this being a JPEG file. 

After opening the file in a hex editor, we can view this header

```text
5c 78 ff e0 00 10 4a 46 49 46 00 01 01 00 00 01
```

The signature for a JPEG file is `FF D8 FF EE`
Correcting the values at the header of the file gives us

```text
ff d8 ff ee 00 10 4a 46 49 46 00 01 01 00 00 01
```

When saving, put the .jpeg file extension on the end.

---

Now we have a functioning JPEG file. One opened the picture is the flag.

```text
picoCTF{r3st0r1ng_th3_by73s_redacted}
```

## **References**
[File Signatures](https://en.wikipedia.org/wiki/List_of_file_signatures)
[Hex editor used](https://freehexeditorneo.com)