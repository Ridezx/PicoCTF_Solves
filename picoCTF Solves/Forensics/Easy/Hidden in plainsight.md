# **picoCTF Challenge:** ***Hidden in plainsight***

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
You’re given a seemingly ordinary JPG image. Something is tucked away out of sight inside the file. Your task is to discover the hidden payload and extract the flag.
Download the jpg image here.
> I do not wish to link the download, but [here](https://play.picoctf.org/practice/challenge/524) is the challenge post on picoCTF

---

> **Hint:** Download the jpg image and read its metadata

---


## **Solution**

### Step 1: Download + View
After downloading the photo, we are greeted with a stereotypical background image you would find on a middle/high school tech related presentation.

Within the files metadata is the embedded flag.

Utilizing a tool called `Exiftool`, we are greeted with this introduction.
```bash
Ridez-picoctf@webshell:~$ exiftool img.jpg
ExifTool Version Number         : 12.40
File Name                       : img.jpg
Directory                       : .
File Size                       : 72 KiB
File Modification Date/Time     : 2025:11:07 19:17:39+00:00
File Access Date/Time           : 2026:04:16 02:23:36+00:00
File Inode Change Date/Time     : 2026:04:16 02:23:27+00:00
File Permissions                : -rw-rw-r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Comment                         : c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9
Image Width                     : 640
Image Height                    : 640
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 640x640
Megapixels                      : 0.410
```

Specifically, our focus should be on the comment:
```text
c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9
```

---

### Step 2: Decode
This is a base64 encrypted string. Since base64 is a symetrical key, running `base64 -d` on the string will return a value.

Inside picoctf webshell,
```bash
echo "c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9" | base64 -d
```

Now we have a new string,
```text
steghide:cEF6endvcmQ
```

---

### Step 3: Decode again because the creator loves making me do tedious work. I hate defense in depth when I'm trying to solve something.
The second half of the string is base64 again. Running it again with,
```bash
echo "cEF6endvcmQ" | base64 -d
```

It returns,
```text
pAzzword
```

---

### Step 4: Steghide View
Now we have a password, but prior we were pointed in the direction of steghide. 
```bash
Ridez-picoctf@webshell:~$ steghide --info img.jpg
"img.jpg":
  format: jpeg
  capacity: 4.0 KB
Try to get information about embedded data ? (y/n) y
Enter passphrase: pAzzword
  embedded file "flag.txt":
    size: 34.0 Byte
    encrypted: rijndael-128, cbc
    compressed: yes
```

---

### Step 5: Steghide Decode
I don't know about you, but `flag.txt` looks rather enticing.
A person may be enticed into immediately reading the file, though the output would not work. We must decode it with the password and overwrite the file.
```bash
Ridez-picoctf@webshell:~$ steghide extract -sf img.jpg -p pAzzword
the file "flag.txt" does already exist. overwrite ? (y/n) y
wrote extracted data to "flag.txt".
Ridez-picoctf@webshell:~$ 
Ridez-picoctf@webshell:~$ cat flag.txt
picoCTF{h1dd3n_1n_1m4g3_e7f5b969}
```

---

### Step 6: Finally, I can view the file
```bash
Ridez-picoctf@webshell:~$ cat flag.txt
picoCTF{h1dd3n_1n_1m4g3_e7f5b969}
```

---

Leaving us with our final flag,
```text
picoCTF{h1dd3n_1n_1m4g3_e7f5b969}
```

## **References**
[Exiftool](https://exiftool.org)