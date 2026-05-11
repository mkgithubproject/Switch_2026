
Perfect set of topics for coding interviews. These patterns appear again and again in DSA rounds at companies like Amazon, Google, Microsoft, and Flipkart.

We’ll learn them in this order:

1. Traversal
2. Prefix Sum
3. Sliding Window
4. Two Pointer
5. Kadane’s Algorithm
6. Dutch National Flag
7. Merge Intervals

And for every topic we’ll do:

* What problem it solves
* Real-life analogy
* Core intuition
* Pattern recognition
* JavaScript examples
* Dry run
* Interview tricks
* Common mistakes
* Problems to practice

---

# 1. Traversal

Traversal is the foundation of almost every array/string problem.

---

# What is Traversal?

Traversal means:

> Visiting every element one by one.

Like walking through all houses in a street.

---

# Real Life Example

Imagine checking attendance in a classroom.

You go seat by seat:

```text
Aman → Ravi → Neha → Kunal
```

That is traversal.

---

# Basic Traversal in JavaScript

```javascript
const arr = [10, 20, 30, 40];

for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);
}
```

Output:

```text
10
20
30
40
```

---

# How It Works Internally

Array in memory:

```text
Index:   0   1   2   3
Value:  10  20  30  40
```

Loop:

```javascript
i = 0 → arr[0] = 10
i = 1 → arr[1] = 20
i = 2 → arr[2] = 30
i = 3 → arr[3] = 40
```

---

# Golden Interview Rule

If interviewer says:

* find
* count
* maximum
* minimum
* check condition
* compare all elements

Your brain should think:

```text
TRAVERSAL
```

---

# Example 1 — Find Sum of Array

Problem:

```text
[1,2,3,4]
```

Output:

```text
10
```

Code:

```javascript
const arr = [1, 2, 3, 4];

let sum = 0;

for (let i = 0; i < arr.length; i++) {
    sum = sum + arr[i];
}

console.log(sum);
```

---

# Dry Run

Initial:

```text
sum = 0
```

Step 1:

```text
sum = 0 + 1 = 1
```

Step 2:

```text
sum = 1 + 2 = 3
```

Step 3:

```text
sum = 3 + 3 = 6
```

Step 4:

```text
sum = 6 + 4 = 10
```

---

# Example 2 — Find Maximum

Problem:

```text
[5,1,9,2]
```

Output:

```text
9
```

Code:

```javascript
const arr = [5, 1, 9, 2];

let max = arr[0];

for (let i = 1; i < arr.length; i++) {

    if (arr[i] > max) {
        max = arr[i];
    }

}

console.log(max);
```

---

# Dry Run

Initial:

```text
max = 5
```

Compare:

```text
1 > 5 ❌
9 > 5 ✅ → max = 9
2 > 9 ❌
```

Final:

```text
9
```

---

# Example 3 — Count Even Numbers

```javascript
const arr = [1, 2, 4, 7, 8];

let count = 0;

for (let i = 0; i < arr.length; i++) {

    if (arr[i] % 2 === 0) {
        count++;
    }

}

console.log(count);
```

Output:

```text
3
```

---

# Most Important Traversal Types

## 1. Left to Right

```javascript
for (let i = 0; i < arr.length; i++)
```

Used most commonly.

---

## 2. Right to Left

```javascript
for (let i = arr.length - 1; i >= 0; i--)
```

Useful for:

* reverse
* suffix problems
* backward checking

---

## 3. Two Arrays Together

```javascript
for (let i = 0; i < arr.length; i++) {
    console.log(arr1[i], arr2[i]);
}
```

---

# Common Interview Problems Using Traversal

---

## Find Minimum

```javascript
function findMin(arr) {

    let min = arr[0];

    for (let i = 1; i < arr.length; i++) {

        if (arr[i] < min) {
            min = arr[i];
        }

    }

    return min;
}
```

---

## Search Element

```javascript
function search(arr, target) {

    for (let i = 0; i < arr.length; i++) {

        if (arr[i] === target) {
            return i;
        }

    }

    return -1;
}
```

---

# Time Complexity

Traversal usually means:

```text
O(n)
```

Because we visit all elements once.

---

# Space Complexity

Usually:

```text
O(1)
```

Because we only use few variables.

---

# Common Mistakes

## Mistake 1

```javascript
i <= arr.length
```

Wrong.

Because last valid index is:

```text
arr.length - 1
```

Correct:

```javascript
i < arr.length
```

---

## Mistake 2

Starting from wrong index.

Example:

```javascript
let max = arr[0];

for (let i = 0; i < arr.length; i++)
```

This compares first element twice.

Better:

```javascript
for (let i = 1; i < arr.length; i++)
```

---

# Pattern Recognition Trick

If problem says:

```text
for every element
```

or

```text
check all values
```

then traversal is involved.

---

# Interview-Level Thinking

Bad candidate:

```text
Writes code directly.
```

Good candidate:

```text
"I will traverse the array once and maintain a variable."
```

Interviewers LOVE hearing this.

---

# Mini Practice

Try these yourself:

1. Count odd numbers
2. Find largest element
3. Find second largest
4. Reverse traversal print
5. Sum of negative numbers
6. Count vowels in string

---

# Tiny Challenge

Solve this:

```text
Input: [2,4,6,8]

Output:
20
```

Using traversal.

---

