# Codeforces Round 1079 (Div 2) — A, B, C Solutions

This round was more focused on logical reasoning and mathematical observation rather than heavy data structures or complex algorithms.

---

# A) Friendly Numbers

**Problem Link:**
[https://codeforces.com/contest/2197/problem/A](https://codeforces.com/contest/2197/problem/A)

---

## Problem Statement

We call a number `y` friendly if:

```
y − d(y) = x
```

Where `d(y)` is the sum of digits of `y`.

---

## Key Observation

We can rewrite the condition as:

```
y = x + d(y)
```

Let `S = d(y)`.

Since:

```
1 ≤ x ≤ 10^9
```

The maximum possible digit sum for numbers around `10^9` is at most **90**
(example: 9,999,999,999 → 90).

So:

* Iterate `S` from `1` to `90`
* Compute `y = x + S`
* Check if `digitSum(y) == S`
* Count valid cases

Time Complexity per test case:
**O(90 × digits)** → effectively constant.

---

## C++ Implementation

```cpp
#include <iostream>
using namespace std;

int digitSum(long long x) {
    int sum = 0;
    while (x > 0) {
        sum += x % 10;
        x /= 10;
    }
    return sum;
}

int main() {
    int t;
    cin >> t;

    while (t--) {
        long long x;
        cin >> x;

        int count = 0;

        for (int s = 1; s <= 90; s++) {
            long long y = x + s;

            if (digitSum(y) == s) {
                count++;
            }
        }

        cout << count << endl;
    }

    return 0;
}
```

---

## Java Implementation

```java
import java.util.Scanner;

public class Main {

    public static int digitSum(long x) {
        int sum = 0;
        while (x > 0) {
            sum += x % 10;
            x /= 10;
        }
        return sum;
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();

        while (t-- > 0) {

            long x = sc.nextLong();
            int count = 0;

            for (int s = 1; s <= 90; s++) {

                long y = x + s;

                if (digitSum(y) == s) {
                    count++;
                }
            }

            System.out.println(count);
        }
    }
}
```

---

## Python Implementation

```python
def digit_sum(x):
    total = 0
    while x > 0:
        total += x % 10
        x //= 10
    return total


t = int(input())

for _ in range(t):
    x = int(input())
    count = 0

    for s in range(1, 91):
        y = x + s
        if digit_sum(y) == s:
            count += 1

    print(count)
```

---

# B) Array and Permutation

**Problem Link:**
[https://codeforces.com/contest/2197/problem/B](https://codeforces.com/contest/2197/problem/B)

---

## Core Idea

Operation allowed:

```
p[i] = p[i+1]
or
p[i+1] = p[i]
```

This means values can **spread to adjacent positions**.

Important insight:

* `p` is a permutation.
* Each value appears exactly once originally.
* If value `x` appears in final array `a`, it must originate from its single position in `p`.

Since spreading only happens to adjacent positions, values cannot "jump over" others.

---

## Key Condition

Let:

```
position[v] = index of v in original permutation p
```

For final array `a` to be valid:

For every adjacent pair:

```
position[a[i]] ≤ position[a[i+1]]
```

If this condition fails anywhere → answer is `NO`.

Otherwise → `YES`.

Time Complexity: **O(n)** per test case.

---

## C++ Implementation

```cpp
#include <iostream>
#include <vector>
using namespace std;

void solve() {
    int n;
    cin >> n;

    vector<int> position(n + 1);

    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        position[x] = i;
    }

    vector<int> a(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    for (int i = 0; i < n - 1; i++) {
        if (position[a[i]] > position[a[i + 1]]) {
            cout << "NO" << endl;
            return;
        }
    }

    cout << "YES" << endl;
}

int main() {
    int t;
    cin >> t;

    while (t--) {
        solve();
    }

    return 0;
}
```

---

## Java Implementation

```java
import java.util.Scanner;

public class Main {

    public static void solve(Scanner sc) {

        int n = sc.nextInt();
        int[] position = new int[n + 1];

        for (int i = 0; i < n; i++) {
            int x = sc.nextInt();
            position[x] = i;
        }

        int[] a = new int[n];

        for (int i = 0; i < n; i++) {
            a[i] = sc.nextInt();
        }

        for (int i = 0; i < n - 1; i++) {
            if (position[a[i]] > position[a[i + 1]]) {
                System.out.println("NO");
                return;
            }
        }

        System.out.println("YES");
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();

        while (t-- > 0) {
            solve(sc);
        }
    }
}
```

---

## Python Implementation

```python
def solve():
    n = int(input())

    position = [0] * (n + 1)

    p_values = list(map(int, input().split()))
    for i in range(n):
        position[p_values[i]] = i

    a = list(map(int, input().split()))

    for i in range(n - 1):
        if position[a[i]] > position[a[i + 1]]:
            print("NO")
            return

    print("YES")


t = int(input())
for _ in range(t):
    solve()
```

---

# C) Came With A Fraction

**Problem Link:**
[https://codeforces.com/contest/2197/problem/C](https://codeforces.com/contest/2197/problem/C)

---

## Key Observation

Bob wins if:

```
q / p = 3 / 2
```

Rewriting:

```
3p = 2q
```

From analysis of game transitions:

Bob wins if and only if:

```
p < q
AND
3p ≥ 2q
```

If both conditions hold → `Bob`
Otherwise → `Alice`

Time Complexity: **O(1)** per test case.

---

## C++ Implementation

```cpp
#include <iostream>
using namespace std;

int main() {
    int t;
    cin >> t;

    while (t--) {
        long long p, q;
        cin >> p >> q;

        if (p < q && 3 * p >= 2 * q) {
            cout << "Bob" << endl;
        } else {
            cout << "Alice" << endl;
        }
    }

    return 0;
}
```

---

## Java Implementation

```java
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int t = sc.nextInt();

        while (t-- > 0) {
            long p = sc.nextLong();
            long q = sc.nextLong();

            if (p < q && 3 * p >= 2 * q) {
                System.out.println("Bob");
            } else {
                System.out.println("Alice");
            }
        }
    }
}
```

---

## Python Implementation

```python
t = int(input())

for _ in range(t):
    p, q = map(int, input().split())

    if p < q and 3 * p >= 2 * q:
        print("Bob")
    else:
        print("Alice")
```

---

# Final Thoughts

This round emphasized:

* Mathematical bounding
* Observation based reasoning
* Understanding propagation constraints
* Translating ratios into linear inequalities


---

If this helped you, consider ⭐ starring the repository.
