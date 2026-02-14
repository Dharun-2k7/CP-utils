# ROAD TO 1200–2 SOLUTIONS

---

All the questions in this mashup contest were based on logical thinking and reasoning skills rather than usage of heavy algorithms.

---
## A) OFFSHORES  
original problem link : https://codeforces.com/contest/2194/problem/B  
difficulty rating : NA  

### The Problem in Plain English

Mark has n banks. He wants to move all his money into one bank.

To move money, he must send exactly x rubles.  
The receiving bank only gets y rubles.  
The difference (x−y) is lost forever as a bank fee.  

Our Goal is to : Find the maximum money he can end up with in any single bank.

---

### Observation

Every time we move money, we lose value. To keep the most money, we should:

- Pick one bank to be the “Target”: This bank will never send money away.
- Drain the other banks: Every other bank should send as many x ruble packages as they can afford.

The Formula:  
If a bank has ai rubles, the number of full transfers it can make is  
⌊ai/x⌋  

Since each transfer results in y rubles arriving at the target:

Money contributed by bank i = (ai/x) ⋅ y

---

### The Concept in 3 Steps

**Step 1: Calculate the “Transferrable Money”**  
Go through all banks and find out how much money can actually be moved out of them (remember: every x sent becomes y received). Sum this up into one global total.

**Step 2: Choose a “Base” Bank**  
Pick one bank to be the destination. This bank never moves its own money, so it loses 0 in fees. Every other bank sends its Money to this base.

**Step 3: Find the Best Base**  
Check every bank as a possible base. The answer is simply:  

[Base Bank’s Original Money] + [Mobile Money from all other banks]

---

My C++ implementation :-  
https://codeforces.com/contest/2194/submission/361963783  

---

### JAVA IMPLEMENTATION

```java
import java.util.*;
public class Main {
    public static void solve(Scanner sc) {
        int n = sc.nextInt();
        long x = sc.nextLong();
        long y = sc.nextLong();
        long[] a = new long[n];
        for (int i = 0; i < n; i++) {
            a[i] = sc.nextLong();
        }
        long total = 0;

        for (int i = 0; i < n; i++) {
            total += (a[i] / x) * y;
        }
        long ans = 0;
        for (int i = 0; i < n; i++) {
            long current = a[i] + (total - (a[i] / x) * y);
            ans = Math.max(ans, current);
        }

        System.out.println(ans);
    }
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();
        while (t-- > 0) {
            solve(sc);
        }
        sc.close();
    }
}
```

---

### PYTHON IMPLEMENTATION

```python
def solve():
    n, x, y = map(int, input().split())
    a = list(map(int, input().split()))

    total = 0

    for i in range(n):
        total += (a[i] // x) * y
    ans = 0

    for i in range(n):
        current = a[i] + (total - (a[i] // x) * y)
        ans = max(ans, current)

    print(ans)

t = int(input())
for _ in range(t):
    solve()
```

---

## B) SEATS  
original problem link : https://codeforces.com/contest/2188/problem/B  
Difficulty rating : 1000  

### The Problem in Plain English

You have a row of seats. Some are already taken (1), others are empty (0).  
You must add more students until no more can sit down (remember: students cannot sit next to each other).

The Goal: Add as few students as possible and find the total count.

To minimize students, you want to leave as much space as possible between them. The most efficient way to block seats is to place a student so they cover three spots: themselves, the seat to the left, and the seat to the right.

---

### Logic to solve

**Case 1:**  
No one is sitting (All 0s)

To block n seats using the minimum number of people, the formula is simply:

(n + 2) / 3  

Example: For 5 seats (00000), we only need 2 people (at seats 2 and 4).

---

**Case 2:**  
Some seats are already taken (1s are present)

**Step 1:** Break the row into segments of zeros using the existing 1s as walls.  
**Step 2:** Check the Middle Gaps.

Gaps between two 1s are "walled" on both sides.  
Because the seats immediately next to the 1s are already blocked, the available gap is smaller.

Formula for middle:  

gap / 3

---

My C++ implementation :  
https://codeforces.com/contest/2188/submission/360529170  

---

### JAVA IMPLEMENTATION

```java
import java.util.*;

public class Main {

    public static void solve(Scanner sc) {
        int n = sc.nextInt();
        String s = sc.next();

        int ones = 0;
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == '1') {
                ones++;
            }
        }

        if (ones == 0) {
            System.out.println((n + 2) / 3);
            return;
        }

        int ans = ones;
        ArrayList<Integer> positions = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == '1') {
                positions.add(i);
            }
        }

        int leftZeros = positions.get(0);
        ans += (leftZeros + 1) / 3;

        for (int i = 0; i < positions.size() - 1; i++) {
            int gap = positions.get(i + 1) - positions.get(i) - 1;
            if (gap > 0) {
                ans += gap / 3;
            }
        }

        int rightZeros = (n - 1) - positions.get(positions.size() - 1);
        ans += (rightZeros + 1) / 3;

        System.out.println(ans);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();
        while (t-- > 0) {
            solve(sc);
        }
        sc.close();
    }
}
```

---

---

## C) SIMPLE REPETITION:
original problem link: https://codeforces.com/contest/2093/problem/C  
Difficulty Rating : 1000  

### OBSERVATIONS FROM PS:

Construct a number by repeating a base number x exactly y times.  
Example: x=3,y=4 ===> 3333.  

Goal: Check if this constructed number is prime.  

If both x>1 and y>1, the answer is always NO.  
Because the number is clearly divisible by x.  

Example:  
3333 = 3 × 1111. It is composite!

---

### Final Logic

Early Exit:  
If x>1 and y>1, print NO immediately. This stops us from dealing with numbers that are too huge for long long.

Construct:  
If we didn’t exit, it means either x=1 or y=1. Build the number using:

a = a * 10 + x

Prime Check:  
Use simple trial division from 2 to a.

---

### why do we use a=a*10+x?

We use this formula to shift the digits and make room for the new one:

a * 10 :  
This shifts every digit one place to the left (the units place becomes the tens place, the tens become hundreds, etc.). It leaves a 0 at the end.

+ x :  
This fills that new 0 at the end with your next digit.

Let’s see it with x=7 and y=3 as example:

Start: a = 7  
Step 1: 7 * 10 + 7 → 70 + 7 = 77  
Step 2: 77 * 10 + 7 → 770 + 7 = 777  

---

My C++ implementation :  
https://codeforces.com/contest/2093/submission/362118151  

---

### JAVA IMPLEMENTATION

```java
import java.util.*;

public class Main {

    static boolean isPrime(long n) {
        if (n < 2) return false;
        for (long i = 2; i * i <= n; i++) {
            if (n % i == 0) return false;
        }
        return true;
    }

    static void solve(Scanner sc) {
        long x = sc.nextLong();
        long y = sc.nextLong();

        if (x > 1 && y > 1) {
            System.out.println("NO");
            return;
        }

        long a = x;
        for (int i = 1; i < y; i++) {
            a = a * 10 + x;
        }

        if (isPrime(a)) {
            System.out.println("YES");
        } else {
            System.out.println("NO");
        }
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

### PYTHON IMPLEMENTATION

```python
def is_prime(n):
    if n < 2:
        return False
    i = 2
    while i * i <= n:
        if n % i == 0:
            return False
        i += 1
    return True


def solve():
    x, y = map(int, input().split())

    if x > 1 and y > 1:
        print("NO")
        return

    a = x
    for _ in range(1, y):
        a = a * 10 + x

    if is_prime(a):
        print("YES")
    else:
        print("NO")


t = int(input())
for _ in range(t):
    solve()
```

---

## D) Binary Typewriter:
original problem link : https://codeforces.com/contest/2103/problem/B  
Difficulty rating : 1100  

### PS IN PLAIN ENGLISH :

You are typing a binary string.

> Pressing a button costs 1.  
> Switching between the ‘0’ and ‘1’ buttons costs 1.  
> You start with your finger on ‘0’.  
> Goal: You can reverse one segment of the string once. Find the minimum cost to type the final string.

---

### THE LOGIC:

Since you have to press a button for every character, the base cost is always n (the length of the string).

The extra cost comes entirely from how many times you switch buttons.

---

### When do you pay extra?

The Start:  
If the first character is 1, you must switch from 0 to 1 immediately (Cost +1).

The Transitions:  
Every time s[i] is different from s[i−1] (e.g., 01 or 10), you must switch (Cost +1).

---

### Step by Step Logic

Step 1: Count the Transitions  
Count how many times the characters change in the string. Let’s call this ans. Also, add 1 to ans if the string starts with 1.

Step 2: The Reversal Power  
Reversing a segment only changes the characters at the two boundaries of that segment.

One reversal can fix at most 2 transitions.

Example:  
In 10101, a reversal can turn it into 11100, turning 4 switches into 2.

Step 3: Calculate the Discount

If ans >= 3: We can reduce the switches by 2.  
If ans == 2: We can reduce the switches by 1.  
If ans < 2: No reduction is possible.

Step 4: Final Formula  
The total cost is:

n + (Final Switches)

---

My C++ implementation :  
https://codeforces.com/contest/2103/submission/362123791  

---

### JAVA IMPLEMENTATION

```java
import java.util.*;

public class Main {

    public static void solve(Scanner sc) {
        int n = sc.nextInt();
        String s = sc.next();

        s = "0" + s;

        int ans = 0;
        char curr = s.charAt(0);

        for (int i = 1; i <= n; i++) {
            char cng = s.charAt(i);
            if (cng != curr) {
                ans++;
                curr = cng;
            }
        }

        if (ans >= 3) {
            System.out.println(n + ans - 2);
        } else if (ans == 2) {
            System.out.println(n + ans - 1);
        } else {
            System.out.println(n + ans);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();
        while (t-- > 0) {
            solve(sc);
        }
        sc.close();
    }
}
```

---

### PYTHON IMPLEMENTATION

```python
def solve():
    n = int(input())
    s = input().strip()

    s = "0" + s

    ans = 0
    curr = s[0]

    for i in range(1, n + 1):
        cng = s[i]
        if cng != curr:
            ans += 1
            curr = cng

    if ans >= 3:
        print(n + ans - 2)
    elif ans == 2:
        print(n + ans - 1)
    else:
        print(n + ans)


t = int(input())
for _ in range(t):
    solve()
```

---

## E) FRUITS :
original problem link : https://codeforces.com/contest/12/problem/C  
Difficulty rating : 1100  

### PS in Plain English :

Valera has a shopping list of m fruits. The seller has n different price tags.  
We don’t know which price tag goes with which fruit yet.

Valera wants to know

Minimum Price: What is the lowest possible total if the price tags are assigned “luckily”?  
Maximum Price: What is the highest possible total if the price tags are assigned “unluckily”?

---

### THE LOGIC

The core idea is simple:

To save money: Assign the cheapest prices to the fruits you buy the most often.  
To spend money: Assign the expensive prices to the fruits you buy the most often.

---

### Step by Step Logic

Step 1: Count the Frequencies  
Use a map/dictionary to count how many times each fruit appears on the list.

Example:  
If the list is orange, orange, apple, our counts are {orange: 2, apple: 1}.

Step 2: Collect the Counts  
We only care about the numbers (frequencies). Put these counts into a list and sort them. We want to know which fruits are Top priority.

Step 3: Sort the Price Tags  
Sort the available price tags from smallest to largest.

Step 4: Greedy Calculation

For Minimum: Match the largest frequencies with the smallest prices.  
For Maximum: Match the largest frequencies with the largest prices.

---

My C++ implementation :  
https://codeforces.com/contest/12/submission/362158890  

---

### JAVA IMPLEMENTATION

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int m = sc.nextInt();

        long[] a = new long[n];
        for (int i = 0; i < n; i++) {
            a[i] = sc.nextLong();
        }

        HashMap<String, Integer> cnt = new HashMap<>();

        for (int i = 0; i < m; i++) {
            String s = sc.next();
            cnt.put(s, cnt.getOrDefault(s, 0) + 1);
        }

        ArrayList<Long> c = new ArrayList<>();
        for (int value : cnt.values()) {
            c.add((long) value);
        }

        while (c.size() < n) {
            c.add(0L);
        }

        Collections.sort(c);
        Arrays.sort(a);

        long mn = 0, mx = 0;

        for (int i = 0; i < n; i++) {
            mx += a[i] * c.get(i);
            mn += a[i] * c.get(n - 1 - i);
        }

        System.out.println(mn + " " + mx);
    }
}
```

---

### PYTHON IMPLEMENTATION

```python
n, m = map(int, input().split())
a = list(map(int, input().split()))

cnt = {}

for _ in range(m):
    s = input().strip()
    cnt[s] = cnt.get(s, 0) + 1

c = list(cnt.values())

while len(c) < n:
    c.append(0)

a.sort()
c.sort()

mn = 0
mx = 0

for i in range(n):
    mx += a[i] * c[i]
    mn += a[i] * c[n - 1 - i]

print(mn, mx)
```

---
