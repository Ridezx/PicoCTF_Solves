# **picoCTF Challenge:** ***vault-door-training***

- [Challenge information](#challenge-information)
- [Solution](#solution)
- [References](#references)

---

## **Challenge information**

<table>
  <tr><td><strong>Level</strong></td><td>🟢 Easy</td></tr>
  <tr><td><strong>Category</strong></td><td>Reverse Engineering</td></tr>
  <tr><td><strong>Event</strong></td><td>picoCTF 2019</td></tr>
  <tr><td><strong>Author</strong></td><td>Mark E. Haase</td></tr>
</table>

---

**Description:**
Your mission is to enter Dr. Evil's laboratory and retrieve the blueprints for his Doomsday Project. The laboratory is protected by a series of locked vault doors. Each door is controlled by a computer and requires a password to open. Unfortunately, our undercover agents have not been able to obtain the secret passwords for the vault doors, but one of our junior agents obtained the source code for each vault's computer! You will need to read the source code for each level to figure out what the password is for that vault door. As a warmup, we have created a replica vault in our training facility.
The source code for the training vault is here: VaultDoorTraining.java

---

> **Hint:** None
- Note: This is the first of a series of problems with the story of entering Dr. Evil's labratory. This is the first of 9 challenges.
---


## **Solution**
The password is stored directly in the source code as a plaintext string. If you can read, you have the password. This is easier than the 'easy' designation implies.

---

```bash
wget https://challenge-files.picoctf.net/c_fickle_tempest/.../VaultDoorTraining.java
```
---
```bash
cat VaultDoorTraining.java
```

```java
import java.util.*;

class VaultDoorTraining {
    public static void main(String args[]) {
        VaultDoorTraining vaultDoor = new VaultDoorTraining();
        Scanner scanner = new Scanner(System.in); 
        System.out.print("Enter vault password: ");
        String userInput = scanner.next();
        String input = userInput.substring("picoCTF{".length(), userInput.length()-1);
        if (vaultDoor.checkPassword(input)) {
            System.out.println("Access granted.");
        } else {
            System.out.println("Access denied!");
        }
    }

    // The password is below. Is it safe to put the password in the source code?
    // What if somebody stole our source code? Then they would know what our
    // password is. Hmm... I will think of some ways to improve the security
    // on the other doors.
    //
    // -Minion #9567
    public boolean checkPassword(String password) {
        return password.equals("w4rm1ng_Up_w1tH_jAv4_000HRedacted");
    }
}
```

**Reasoning:**
Its obvious why this is a problem, so I'll explain the code instead. 
```java
String input = userInput.substring("picoCTF{".length(), userInput.length()-1);
if (vaultDoor.checkPassword(input)) {
            System.out.println("Access granted.");
        } else {
            System.out.println("Access denied!");
        }
```
This checks for if the clear-text password has been wrapped around the usual `picoCTF{}` encapsulation.

---


### **Flag**

```
picoCTF{w4rm1ng_Up_w1tH_jAv4_000HRedacted}
```
---

> Note: The vault password is the flag for this challenge and is used to complete the next challenge.
- [Next Challenge: Vault Door 1](../Medium/Vault-Door-1.md)


## **References**
None