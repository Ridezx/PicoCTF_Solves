# **picoCTF Challenge:** ***ASCII Numbers***

- [Challenge information](#challenge-information)
- [Solution](#solution)
- [References](#references)

---

## **Challenge information**

<table>
  <tr><td><strong>Level</strong></td><td>🟡 Medium</td></tr>
  <tr><td><strong>Category</strong></td><td>General Skills</td></tr>
  <tr><td><strong>Event</strong></td><td>picoGym Exclusive</td></tr>
  <tr><td><strong>Author</strong></td><td>LT 'SYREAL' Jones</td></tr>
</table>

---

**Description:**
Convert the following string of ASCII numbers into a readable string:
0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x35 0x63 0x31 0x31 0x5f 0x6e 0x30 0x5f 0x71 0x75 0x33 0x35 0x37 0x31 0x30 0x6e 0x35 0x5f 0x31 0x6c 0x6c 0x5f 0x74 0x33 0x31 0x31 0x5f 0x79 0x33 0x5f 0x6e 0x30 0x5f 0x6c 0x31 0x33 0x35 0x5f 0x34 0x34 0x35 0x64 0x34 0x31 0x38 0x30 0x7d

---

> **Hint:** CyberChef is a great tool for any encoding but especially ASCII. Try CyberChef's 'From Hex' function

---


## **Solution**
The description of this challenge provides all the necessary information. Immediately, I recognize this as hex encoded ASCII text. The hint guides us to a tool that automatically does it for us, but where is the fun in that? With [this ASCII table](https://ss64.com/ascii.html), we can find out what hex returns what character.

### **Hex conversion table**

| Decimal | Hex | Character |
|---|---|---|
| 112 | 0x70 | p |
| 105 | 0x69 | i |
| 99 | 0x63 | c |
| 111 | 0x6F | o |
| 67 | 0x43 | C |
| 84 | 0x54 | T |
| 70 | 0x46 | F |
| 123 | 0x7B | { |
| 52 | 0x34 | 4 |
| 53 | 0x35 | 5 |
| 99 | 0x63 | c |
| 49 | 0x31 | 1 |
| 49 | 0x31 | 1 |
| 95 | 0x5F | _ |
| 110 | 0x6E | n |
| 48 | 0x30 | 0 |
| 95 | 0x5F | _ |
| 113 | 0x71 | q |
| 117 | 0x75 | u |
| 51 | 0x33 | 3 |
| 53 | 0x35 | 5 |
| 55 | 0x37 | 7 |
| 49 | 0x31 | 1 |
| 48 | 0x30 | 0 |
| 110 | 0x6E | n |
| 53 | 0x35 | 5 |
| 95 | 0x5F | _ |
| 49 | 0x31 | 1 |
| 108 | 0x6C | l |
| 108 | 0x6C | l |
| 95 | 0x5F | _ |
| 116 | 0x74 | t |
| 51 | 0x33 | 3 |
| 49 | 0x31 | 1 |
| 49 | 0x31 | 1 |
| 95 | 0x5F | _ |
| 121 | 0x79 | y |
| 51 | 0x33 | 3 |
| 95 | 0x5F | _ |
| 110 | 0x6E | n |
| 48 | 0x30 | 0 |
| 95 | 0x5F | _ |
| 108 | 0x6C | l |
| 49 | 0x31 | 1 |
| 51 | 0x33 | 3 |
| 53 | 0x35 | 5 |
| 95 | 0x5F | _ |
| 52 | 0x34 | 4 |
| 52 | 0x34 | 4 |
| 53 | 0x35 | 5 |
| 100 | 0x64 | d |
| 52 | 0x34 | 4 |
| 49 | 0x31 | 1 |
| 56 | 0x38 | 8 |
| 48 | 0x30 | 0 |
| 125 | 0x7D | } |

---

### **Assembled flag**

Reading each character from the table in order:

```
p i c o C T F { 4 5 c 1 1 _ n 0 _ q u 3 5 7 1 0 n 5 _ 1 l l _ t 3 1 1 _ y 3 _ n 0 _ l 1 3 5 _ 4 4 5 d 4 1 8 0 }
```

---

Now to remove the spaces and submit our flag.

### **Flag**

```
picoCTF{45c11_n0_qu35710n5_1ll_t311_y3_n0_l135_445d4180}
```

## **References**
[CyberChef](https://gchq.github.io/CyberChef/)
[ASCII table](https://ss64.com/ascii.html)