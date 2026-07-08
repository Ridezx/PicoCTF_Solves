# **picoCTF Challenge:** ***Perceptron Train XOR***

- [Challenge information](#challenge-information)
- [Solution](#solution)
- [References](#references)

---

## **Challenge information**

<table>
  <tr><td><strong>Level</strong></td><td>🟢 Easy</td></tr>
  <tr><td><strong>Category</strong></td><td>Artificial Intelligence</td></tr>
  <tr><td><strong>Event</strong></td><td>N/A</td></tr>
  <tr><td><strong>Author</strong></td><td>LT 'syreal' Jones</td></tr>
</table>

---

**Description:**
Watch a perceptron learn in real time on XOR data using the classic update rule: only misclassified points trigger updates, with no weight decay. Because XOR is not linearly separable, a single perceptron cannot hit 100% accuracy. Reach 75% accuracy to prove you understand the limitation and reveal the flag.

---

**Connect via xxx:**
```bash
xxx
```

---

> **Hint 1:** You get 16 updates per run, and correctly classified points do not change the model in the classic rule.
> **Hint 2:** The learning-rate sweep is wide (0.02 through 20.0), so very high values can swing the boundary aggressively.
> **Hint 3:** XOR cannot be perfectly separated by one line; 75% is the best achievable target for this setup.
> **Hint 4:** Use the slider to sweep the range and watch how the decision boundary moves between misclassification updates.

---


## **Solution**


## **References**