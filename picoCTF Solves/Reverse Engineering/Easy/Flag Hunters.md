# **picoCTF Challenge:** ***Flag Hunters***

- [Challenge information](#challenge-information)
- [Solution](#solution)
- [References](#references)

---

## **Challenge information**

<table>
  <tr><td><strong>Level</strong></td><td>🟢 Easy</td></tr>
  <tr><td><strong>Category</strong></td><td>Reverse Engineering</td></tr>
  <tr><td><strong>Event</strong></td><td>picoCTF 2025</td></tr>
  <tr><td><strong>Author</strong></td><td>syreal</td></tr>
</table>

---

**Description:**
Lyrics jump from verses to the refrain kind of like a subroutine call. There's a hidden refrain this program doesn't print by default. Can you get it to print it? There might be something in it for you.
The program's source code can be downloaded here.
Additional details will be available after launching your challenge instance.
> I do not wish to link the download, but [here](https://play.picoctf.org/practice/challenge/472) is the challenge post on picoCTF

---

> **Hint:** This program can easily get into undefined states. Don't be shy about Ctrl-C.
> **Hint:** Unsanitized user input is always good, right?
> **Hint:** Is there any syntax that is ripe for subversion?

---

## **Solution**

This challenge involves unsanitized input injection. This is a vulnerability where user input is passed directly into a program's logic without being validated. If the program uses a delimiter like `;` to separate commands, and user input is never checked for that character, an attacker can inject additional commands by simply including them after a semicolon. It similar to command injection with the use of pipes (`|`)

---

### **Step 1 — Downloading and reading the code**
Get the file:
```bash
wget https://challenge-files.picoctf.net/c_verbal_sleep/1f8519360cee8abb5571512dfc4edbe42bdb5b91a96e200d2fb58302a312726b/lyric-reader.py
```

Read the file:
```bash
cat lyric-reader.py
```

Code:
```python
import re
import time

# Read in flag from file
flag = open('flag.txt', 'r').read()

secret_intro = \
'''Pico warriors rising, puzzles laid bare,
Solving each challenge with precision and flair.
With unity and skill, flags we deliver,
The ether's ours to conquer, '''\
+ flag + '\n'

song_flag_hunters = secret_intro +\
```

**Reasoning:**
Reading the source before connecting to the server is standard reverse engineering practice. We need to know how the program works before we interact with it. First, the `secret_intro` variable contains the flag, which will be appended directly to the end of the intro verse. Second, `secret_intro` is prepended to `song_flag_hunters`, meaning the flag exists at the very beginning of the song. The reader just never starts there.

---

### **Step 2 — Understanding the logic**

The `reader` function controls which line of the song is currently being printed. It tracks position using a line index pointer and processes each line for special commands.

```python
reader(song_flag_hunters, '[VERSE1]')
```

The reader is called with `[VERSE1]` as the start label, which means it begins printing from Verse 1. This skips over `secret_intro` entirely. The flag is sitting at line 0, but the reader never goes there under normal execution.

---

### **Step 3 — Injection point**

The program handles several special commands by splitting each line on `;`:

```python
for line in song_lines[lip].split(';'):
```

The `RETURN` command jumps the reader to a specific line number:

```python
elif re.match(r"RETURN [0-9]+", line):
    lip = int(line.split()[1])
```

The `CROWD` handler takes user input and writes it directly back into the song without any sanitization:

```python
elif re.match(r"CROWD.*", line):
    crowd = input('Crowd: ')
    song_lines[lip] = 'Crowd: ' + crowd
    lip += 1
```

**Reasoning:**
The `;` delimiter is the vulnerability. The program splits every line on `;` before checking what command it contains. If our input contains a `;`, anything after it will be treated as a separate command. By entering `some_string;RETURN 0`, the program processes `some_string` as the crowd response, then processes `RETURN 0` as a command.

---

### **Step 4 — Injection**

```bash
Ridez-academy@webshell:~$ nc verbal-sleep.picoctf.net 56906

Command line wizards, we're starting it right,
Spawning shells in the terminal, hacking all night.
Scripts and searches, grep through the void,
Every keystroke, we're a cypher's envoy.
Brute force the lock or craft that regex,
Flag on the horizon, what challenge is next?

We're flag hunters in the ether, lighting up the grid,
No puzzle too dark, no challenge too hid.
With every exploit we trigger, every byte we decrypt,
We're chasing that victory, and we'll never quit.
Crowd: 
```

The program pauses at `CROWD`, waiting for input. 
Inject:

```bash
Crowd: some_string;RETURN 0
```

The reader processes `some_string` as the crowd input, then executes `RETURN 0`, jumping back to the beginning of the song:

```bash
Pico warriors rising, puzzles laid bare,
Solving each challenge with precision and flair.
With unity and skill, flags we deliver,
The ether's ours to conquer, picoCTF{70637h3r_f0r3v3r_Redacted}
```

**Reasoning:**
`RETURN 0` sends the line index pointer to position 0. The reader then prints each line sequentially. The flag is concatenated directly onto the end of the intro verse.

```
picoCTF{70637h3r_f0r3v3r_Redacted}
```

---
 
## **References**
- [Input sanitization - OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)