# Recap: Modular and Integer Arithmetic
CODEFORCES BLOG LINK : https://codeforces.com/blog/entry/149808
This is a comprehensive recap of modular arithmetic and integer properties, useful for beginners and as a refresher for competitive programming.

---

## 1. Basic Observations
The modulo operator (`%`) provides the remainder of a division.

* **Divisibility:** If $a \pmod b = 0$, then $a$ is divisible by $b$. This is the foundation for solving number theory problems and constraint checking.
* **Parity:** Determining if a number is even or odd.
    * `n % 2 == 0` $\implies$ **Even**
    * `n % 2 == 1` $\implies$ **Odd**
    * Whenever a problem involves alternating behavior, parity, or splitting elements into two groups, modulo 2 is usually the first thing worth checking.

## 2. Cyclic Behavior
One important property of modulo is that it naturally creates cycles. The values of $n \pmod 5$, for instance, repeat as $0, 1, 2, 3, 4, 0, 1, 2, 3, 4, \dots$



### Real-world Examples:
* **Circular Arrays:** If an array has size $n$ and an index $i$ goes out of bounds, using `i % n` wraps it back into the valid range.
* **Days of the Week:** They repeat every 7 days, so it is exactly modular arithmetic ($N \pmod 7$ would give us the day even if the $N$ value is very large).

---

## 3. Modulo Properties and Large Numbers
Two properties of modulo are used constantly in competitive programming:

1. $(a + b) \pmod m = ((a \pmod m) + (b \pmod m)) \pmod m$
2. $(a \times b) \pmod m = ((a \pmod m) \times (b \pmod m)) \pmod m$

These identities allow us to reduce numbers at every step instead of working with extremely large values. This is crucial when problems ask for answers modulo a large number such as $10^9 + 7$. Multiplying first and taking modulo at the end often causes overflow, while applying modulo during intermediate steps keeps everything safe.

### Coding Example: Avoiding Memory/Overflow Errors
The following code returns **ML (Memory Limit error)** due to overflow while calculating the power:

```python
import math
tc = int(input())
mod = 10**9 + 7
for i in range(tc):
    m = int(input())
    p = (m - 1)**2
    r = pow(2, p)  # The result is too large to store in memory
    print(r % mod)

The Optimized Version: By reducing both numbers modulo m first, then multiplying, and finally applying modulo again, the result remains correct while staying within safe limits.
Python

tc = int(input())
mod = 10**9 + 7
for i in range(tc):
    m = int(input())
    p = (m - 1) * (m - 1)
    # Using the 3rd argument of pow() handles modular exponentiation
    print(pow(2, p, mod)) 

```
4. Negative Numbers and Modulo

Modulo with negative numbers is handled differently depending on the language. Mathematically, −7(mod3) equals 2.
Language	Result of -7 % 3
Python	2
C++ / Java	-1

The Safe Expression: To avoid surprises and ensure a value in the range [0,m−1] across any language, use: $$ (a \pmod m + m) \pmod m $$
5. Digit Extraction

Modulo is also heavily used in digit manipulation problems.

    Extract the last digit: n % 10

    Remove the last digit: n // 10 (integer division)

Example: Repeatedly applying these operations to 12345 extracts digits from right to left: 5, 4, 3, 2, 1. This pattern appears in problems involving digit sums, palindromes, and reversing numbers.
6. Common Mistakes

    Confusing Division with Modulo: Mixing them incorrectly in expressions without proper parentheses.

    Ignoring Negative Behavior: This can silently break solutions in languages like C++ or Java.

    Late Modulo: Applying modulo only at the end of a long calculation often leads to overflow and incorrect results. Always apply modulo at every intermediate step.

Practice Problems

    Happy Number

    Add Digits

    Fizz Buzz

    Power of Two

This post was written while revisiting the modulo and integer arithmetic sections on REPOVIVE. Thanks to Shayan for building and maintaining such a useful resource.
