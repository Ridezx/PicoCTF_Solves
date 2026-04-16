# **picoCTF Challenge:** ***The Numbers***

- [Challenge information](#challenge-information)
- [Solution](#solution)

---

## **Challenge information**

<table>
  <tr><td><strong>Level</strong></td><td>🟢 Easy</td></tr>
  <tr><td><strong>Category</strong></td><td>Cryptography</td></tr>
  <tr><td><strong>Event</strong></td><td>picoCTF 2019</td></tr>
  <tr><td><strong>Author</strong></td><td>Danny</td></tr>
</table>

---

**Description:**
The numbers... what do they mean?
numbers.png
> I do not wish to link the download, but [here](https://play.picoctf.org/practice/challenge/68) is the challenge post on picoCTF

---

> **Hint:** The flag is in the format PICOCTF{}

---


## **Solution**
Opening the png gives up the numbers:
```text
16 9 3 15 3 20 6 { 20 8 5 14 21 13 2 5 18 19 13 1 19 15 14 }
```
The numbers are associated to what number they are in the alphabet. The 16th letter of the alphabet is `p`, 9th is `i`, you get the gist.


### Table
# Alphabet Position Conversion
| Number | Letter |
| :--- | :--- |
| 16 | P |
| 9 | I |
| 3 | C |
| 15 | O |
| 3 | C |
| 20 | T |
| 6 | F |
| { | { |
| 20 | T |
| 8 | H |
| 5 | E |
| 14 | N |
| 21 | U |
| 13 | M |
| 2 | B |
| 5 | E |
| 18 | R |
| 19 | S |
| 13 | M |
| 1 | A |
| 19 | S |
| 15 | O |
| 14 | N |
| } | } |

### Flag
`PICOCTF{THENUMBERSMASON}`