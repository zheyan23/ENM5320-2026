# Practice Exercises: Mathematical Fundamentals

**Related Material:** [Analysis.html](Analysis.html)

These exercises are designed to help you practice the concepts covered in the Mathematical Fundamentals notes. 

---

## Exercise Set 1: Vector Norms

### Exercise 1.1
Given $\mathbf{v} = [2, -3, 6, -1]^T$, compute:

a) $\|\mathbf{v}\|_1$

b) $\|\mathbf{v}\|_2$

c) $\|\mathbf{v}\|_\infty$

**Solution:**

a) $\|\mathbf{v}\|_1 = |2| + |-3| + |6| + |-1| = 2 + 3 + 6 + 1 = 12$

b) $\|\mathbf{v}\|_2 = \sqrt{2^2 + (-3)^2 + 6^2 + (-1)^2} = \sqrt{4 + 9 + 36 + 1} = \sqrt{50} = 5\sqrt{2} \approx 7.071$

c) $\|\mathbf{v}\|_\infty = \max\{|2|, |-3|, |6|, |-1|\} = 6$

---

### Exercise 1.2
Verify the inequality $\|\mathbf{v}\|_\infty \leq \|\mathbf{v}\|_2 \leq \|\mathbf{v}\|_1$ for $\mathbf{v} = [1, -4, 2]^T$.

**Solution:**

Compute each norm:
- $\|\mathbf{v}\|_1 = |1| + |-4| + |2| = 7$
- $\|\mathbf{v}\|_2 = \sqrt{1^2 + (-4)^2 + 2^2} = \sqrt{1 + 16 + 4} = \sqrt{21} \approx 4.583$
- $\|\mathbf{v}\|_\infty = \max\{|1|, |-4|, |2|\} = 4$

Verify: $4 \leq 4.583 \leq 7$ ✓

---

