# Chapter 3 — Practice Problems (Java)

A complete collection of practice programs covering **Decision Making**, **Loops**, and **Arrays** — the core building blocks from Chapter 3. Each problem includes a problem statement, algorithm, Java code, sample input/output, explanation, and time complexity (where relevant).

---

# Part A: Decision Making

---

## A1. Check Even or Odd

### Problem Statement

Given an integer, determine whether it is even or odd.

### Algorithm

1. Read the integer `n`.
2. Compute `n % 2`.
3. If the remainder is `0`, print "Even"; otherwise print "Odd".

### Java Code

```java
import java.util.Scanner;

public class EvenOdd {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter a number: ");
        int n = sc.nextInt();

        if (n % 2 == 0) {
            System.out.println(n + " is Even");
        } else {
            System.out.println(n + " is Odd");
        }
    }
}
```

### Sample Input/Output

```
Input: 7
Output: 7 is Odd

Input: 10
Output: 10 is Even
```

### Explanation

The modulus operator `%` returns the remainder of division by 2. Any number perfectly divisible by 2 (remainder 0) is even; otherwise it's odd. This also works correctly for negative numbers in Java, since `-4 % 2 == 0`.

### Time Complexity

O(1) — a single arithmetic check regardless of input size.

---

## A2. Find the Largest of Three Numbers

### Problem Statement

Given three numbers, find and print the largest among them.

### Algorithm

1. Read three numbers `a`, `b`, `c`.
2. Compare `a` and `b`; keep the larger as `max`.
3. Compare `max` and `c`; update `max` if `c` is larger.
4. Print `max`.

### Java Code

```java
import java.util.Scanner;

public class LargestOfThree {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter three numbers: ");
        int a = sc.nextInt(), b = sc.nextInt(), c = sc.nextInt();

        int max = (a > b) ? a : b;
        max = (max > c) ? max : c;

        System.out.println("Largest = " + max);
    }
}
```

### Sample Input/Output

```
Input: 12 45 30
Output: Largest = 45
```

### Explanation

Using nested ternary comparisons avoids writing multiple `if-else` blocks. First we find the larger of `a` and `b`, then compare that result with `c` to get the overall maximum. This can also be done using `Math.max(a, Math.max(b, c))`.

### Time Complexity

O(1) — a constant number of comparisons regardless of input.

---

## A3. Grade Calculator

### Problem Statement

Given a student's marks (out of 100), assign a grade based on the following scale:

- 90–100: A
- 75–89: B
- 60–74: C
- 40–59: D
- Below 40: F

### Algorithm

1. Read `marks`.
2. Use a chain of `if-else if` conditions from highest to lowest range.
3. Print the corresponding grade.

### Java Code

```java
import java.util.Scanner;

public class GradeCalculator {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter marks: ");
        int marks = sc.nextInt();
        char grade;

        if (marks >= 90) {
            grade = 'A';
        } else if (marks >= 75) {
            grade = 'B';
        } else if (marks >= 60) {
            grade = 'C';
        } else if (marks >= 40) {
            grade = 'D';
        } else {
            grade = 'F';
        }

        System.out.println("Grade: " + grade);
    }
}
```

### Sample Input/Output

```
Input: 82
Output: Grade: B

Input: 35
Output: Grade: F
```

### Explanation

The conditions are checked in descending order of range, so the first true condition determines the grade. Order matters here — checking from the lowest boundary upward would give incorrect results.

### Time Complexity

O(1) — at most 5 comparisons, independent of input size.

---

## A4. Leap Year Checker

### Problem Statement

Given a year, determine whether it is a leap year. A year is a leap year if it is divisible by 4, but not by 100, unless it is also divisible by 400.

### Algorithm

1. Read the year `y`.
2. Check: `(y % 4 == 0 && y % 100 != 0) || (y % 400 == 0)`.
3. If true, print "Leap Year"; else print "Not a Leap Year".

### Java Code

```java
import java.util.Scanner;

public class LeapYearChecker {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter a year: ");
        int y = sc.nextInt();

        boolean isLeap = (y % 4 == 0 && y % 100 != 0) || (y % 400 == 0);

        if (isLeap) {
            System.out.println(y + " is a Leap Year");
        } else {
            System.out.println(y + " is Not a Leap Year");
        }
    }
}
```

### Sample Input/Output

```
Input: 2024
Output: 2024 is a Leap Year

Input: 1900
Output: 1900 is Not a Leap Year

Input: 2000
Output: 2000 is a Leap Year
```

### Explanation

The rule has three parts: divisible by 4 (basic rule), NOT divisible by 100 (century exception), OR divisible by 400 (exception to the exception). `1900` fails because it's divisible by 100 but not 400; `2000` passes because it's divisible by 400.

### Time Complexity

O(1) — three modulus operations regardless of input size.

---

## A5. Calculator Using `switch`

### Problem Statement

Build a simple calculator that performs addition, subtraction, multiplication, and division based on a user-provided operator.

### Algorithm

1. Read two numbers `a`, `b` and an operator `op`.
2. Use a `switch` statement on `op` to select the operation.
3. Handle division by zero as a special case.
4. Print the result.

### Java Code

```java
import java.util.Scanner;

public class SwitchCalculator {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter first number: ");
        double a = sc.nextDouble();
        System.out.print("Enter operator (+, -, *, /): ");
        char op = sc.next().charAt(0);
        System.out.print("Enter second number: ");
        double b = sc.nextDouble();

        double result;
        switch (op) {
            case '+':
                result = a + b;
                break;
            case '-':
                result = a - b;
                break;
            case '*':
                result = a * b;
                break;
            case '/':
                if (b == 0) {
                    System.out.println("Error: Division by zero");
                    return;
                }
                result = a / b;
                break;
            default:
                System.out.println("Invalid operator");
                return;
        }

        System.out.println("Result = " + result);
    }
}
```

### Sample Input/Output

```
Input: 10 + 5
Output: Result = 15.0

Input: 10 / 0
Output: Error: Division by zero
```

### Explanation

The `switch` statement matches the character operator against each `case` and executes the matching block. `break` prevents fall-through to subsequent cases. A `default` case handles invalid operators, and division by zero is explicitly checked before performing the operation to avoid `Infinity`/`NaN` results.

### Time Complexity

O(1) — a constant-time branch selection and single arithmetic operation.

---

# Part B: Loops

---

## B1. Print Numbers (1 to N)

### Problem Statement

Print all numbers from 1 to `n`.

### Algorithm

1. Read `n`.
2. Loop `i` from `1` to `n`, printing each value.

### Java Code

```java
import java.util.Scanner;

public class PrintNumbers {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter n: ");
        int n = sc.nextInt();

        for (int i = 1; i <= n; i++) {
            System.out.print(i + " ");
        }
    }
}
```

### Sample Input/Output

```
Input: 5
Output: 1 2 3 4 5
```

### Explanation

A simple counting loop starting at 1, incrementing by 1 each time, and terminating once `i` exceeds `n`.

### Time Complexity

O(n) — the loop runs exactly `n` times.

---

## B2. Sum of First N Numbers

### Problem Statement

Compute the sum of the first `n` natural numbers.

### Algorithm

1. Read `n`.
2. Initialize `sum = 0`.
3. Loop `i` from `1` to `n`, adding `i` to `sum` each time.
4. Print `sum`.

### Java Code

```java
import java.util.Scanner;

public class SumOfN {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter n: ");
        int n = sc.nextInt();

        int sum = 0;
        for (int i = 1; i <= n; i++) {
            sum += i;
        }

        System.out.println("Sum = " + sum);
    }
}
```

### Sample Input/Output

```
Input: 5
Output: Sum = 15
```

### Explanation

Each iteration adds the current number to a running total. This can also be computed directly using the formula `n * (n + 1) / 2` in O(1) time, but the loop-based approach demonstrates iterative accumulation.

### Time Complexity

O(n) using the loop; O(1) using the direct formula.

---

## B3. Factorial of a Number

### Problem Statement

Compute the factorial of a non-negative integer `n` (`n! = n × (n-1) × ... × 1`, with `0! = 1`).

### Algorithm

1. Read `n`.
2. Initialize `fact = 1`.
3. Loop `i` from `1` to `n`, multiplying `fact` by `i` each time.
4. Print `fact`.

### Java Code

```java
import java.util.Scanner;

public class Factorial {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter n: ");
        int n = sc.nextInt();

        long fact = 1;
        for (int i = 1; i <= n; i++) {
            fact *= i;
        }

        System.out.println(n + "! = " + fact);
    }
}
```

### Sample Input/Output

```
Input: 5
Output: 5! = 120

Input: 0
Output: 0! = 1
```

### Explanation

`long` is used instead of `int` since factorials grow extremely fast (e.g., `20!` already exceeds `int` range). The loop starts at 1 (multiplying by 1 has no effect) and multiplies cumulatively up to `n`. If `n = 0`, the loop doesn't execute at all, leaving `fact = 1`, which correctly represents `0! = 1`.

### Time Complexity

O(n) — one multiplication per iteration.

---

## B4. Fibonacci Series

### Problem Statement

Print the first `n` terms of the Fibonacci series (`0, 1, 1, 2, 3, 5, 8, ...`), where each term is the sum of the two preceding ones.

### Algorithm

1. Read `n`.
2. Initialize `a = 0`, `b = 1`.
3. Print `a` and `b` (handling edge cases for `n = 0` or `1`).
4. Loop from the 3rd term to `n`, computing `next = a + b`, then shifting `a = b`, `b = next`.

### Java Code

```java
import java.util.Scanner;

public class FibonacciSeries {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter number of terms: ");
        int n = sc.nextInt();

        int a = 0, b = 1;
        for (int i = 1; i <= n; i++) {
            System.out.print(a + " ");
            int next = a + b;
            a = b;
            b = next;
        }
    }
}
```

### Sample Input/Output

```
Input: 8
Output: 0 1 1 2 3 5 8 13
```

### Explanation

We maintain two variables (`a`, `b`) representing the last two terms. In each iteration, we print `a` (the current term), then compute the next term and shift the window forward. This avoids using an array, achieving O(1) space.

### Time Complexity

O(n) time, O(1) space (iterative approach). A naive recursive approach would take O(2ⁿ) time.

---

## B5. Check Prime Number

### Problem Statement

Determine whether a given number `n` is prime (divisible only by 1 and itself, and greater than 1).

### Algorithm

1. Read `n`.
2. If `n <= 1`, it's not prime.
3. Loop `i` from `2` to `√n`; if `n % i == 0` for any `i`, `n` is not prime.
4. If no divisor is found, `n` is prime.

### Java Code

```java
import java.util.Scanner;

public class PrimeCheck {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter a number: ");
        int n = sc.nextInt();

        boolean isPrime = true;
        if (n <= 1) {
            isPrime = false;
        } else {
            for (int i = 2; i * i <= n; i++) {
                if (n % i == 0) {
                    isPrime = false;
                    break;
                }
            }
        }

        System.out.println(n + (isPrime ? " is Prime" : " is Not Prime"));
    }
}
```

### Sample Input/Output

```
Input: 29
Output: 29 is Prime

Input: 15
Output: 15 is Not Prime
```

### Explanation

Instead of checking divisibility up to `n`, we only check up to `√n`, since if `n` has a factor larger than `√n`, it must also have a corresponding factor smaller than `√n`. This significantly reduces the number of checks needed. The loop breaks early as soon as a divisor is found.

### Time Complexity

O(√n) — a major optimization over the naive O(n) approach.

---

## B6. Armstrong Number

### Problem Statement

Check whether a number is an **Armstrong number** — a number equal to the sum of its own digits, each raised to the power of the number of digits (e.g., `153 = 1³ + 5³ + 3³`).

### Algorithm

1. Read `n`, store original value.
2. Count the number of digits, `d`.
3. Extract each digit, raise it to the power `d`, and add to `sum`.
4. Compare `sum` with the original number.

### Java Code

```java
import java.util.Scanner;

public class ArmstrongNumber {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter a number: ");
        int n = sc.nextInt();
        int original = n;

        int digits = String.valueOf(n).length();
        int sum = 0;

        while (n > 0) {
            int digit = n % 10;
            sum += Math.pow(digit, digits);
            n /= 10;
        }

        if (sum == original) {
            System.out.println(original + " is an Armstrong Number");
        } else {
            System.out.println(original + " is Not an Armstrong Number");
        }
    }
}
```

### Sample Input/Output

```
Input: 153
Output: 153 is an Armstrong Number

Input: 123
Output: 123 is Not an Armstrong Number
```

### Explanation

Each digit is extracted using `n % 10`, then `n` is reduced using `n /= 10`. Every digit is raised to the power equal to the total digit count and accumulated. If the final sum matches the original number, it's an Armstrong number.

### Time Complexity

O(d), where `d` is the number of digits in `n` (effectively O(log₁₀ n)).

---

## B7. Palindrome Check (Number)

### Problem Statement

Check whether a number reads the same forwards and backwards (e.g., `121`, `1331`).

### Algorithm

1. Read `n`, store original value.
2. Reverse the digits of `n` to form `reversed`.
3. Compare `reversed` with the original number.

### Java Code

```java
import java.util.Scanner;

public class PalindromeNumber {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter a number: ");
        int n = sc.nextInt();
        int original = n;
        int reversed = 0;

        while (n > 0) {
            int digit = n % 10;
            reversed = reversed * 10 + digit;
            n /= 10;
        }

        if (reversed == original) {
            System.out.println(original + " is a Palindrome");
        } else {
            System.out.println(original + " is Not a Palindrome");
        }
    }
}
```

### Sample Input/Output

```
Input: 121
Output: 121 is a Palindrome

Input: 123
Output: 123 is Not a Palindrome
```

### Explanation

We build the reversed number digit by digit: each extracted digit is appended to `reversed` by shifting existing digits left (`reversed * 10`) and adding the new digit. Comparing the fully reversed number to the original tells us if it's a palindrome.

### Time Complexity

O(d), where `d` is the number of digits in `n`.

---

## B8. Reverse a Number

### Problem Statement

Reverse the digits of a given integer (e.g., `1234` → `4321`).

### Algorithm

1. Read `n`.
2. Initialize `reversed = 0`.
3. While `n > 0`: extract last digit, append to `reversed`, remove last digit from `n`.
4. Print `reversed`.

### Java Code

```java
import java.util.Scanner;

public class ReverseNumber {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter a number: ");
        int n = sc.nextInt();
        int reversed = 0;

        while (n > 0) {
            int digit = n % 10;
            reversed = reversed * 10 + digit;
            n /= 10;
        }

        System.out.println("Reversed number: " + reversed);
    }
}
```

### Sample Input/Output

```
Input: 6789
Output: Reversed number: 9876
```

### Explanation

Same digit-extraction technique as the palindrome check — repeatedly peel off the last digit using `% 10` and rebuild the number in reverse order by shifting the accumulator left before adding each new digit.

### Time Complexity

O(d), where `d` is the number of digits.

---

## B9. Multiplication Table

### Problem Statement

Print the multiplication table of a given number `n` up to 10 terms.

### Algorithm

1. Read `n`.
2. Loop `i` from `1` to `10`.
3. Print `n x i = n*i`.

### Java Code

```java
import java.util.Scanner;

public class MultiplicationTable {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter a number: ");
        int n = sc.nextInt();

        for (int i = 1; i <= 10; i++) {
            System.out.println(n + " x " + i + " = " + (n * i));
        }
    }
}
```

### Sample Input/Output

```
Input: 5
Output:
5 x 1 = 5
5 x 2 = 10
...
5 x 10 = 50
```

### Explanation

A straightforward counting loop from 1 to 10, printing each multiple of `n` in a formatted line.

### Time Complexity

O(1) — the loop always runs a fixed 10 times, regardless of `n`.

---

## B10. Pattern Printing (Stars and Numbers)

### Problem Statement

Print common triangular patterns using stars and numbers, given the number of rows `n`.

### Algorithm (Star Triangle)

1. Read `n`.
2. Outer loop `i` from `1` to `n` (controls rows).
3. Inner loop `j` from `1` to `i` (controls stars per row).
4. Print a newline after each row.

### Java Code

```java
import java.util.Scanner;

public class PatternPrinting {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter number of rows: ");
        int n = sc.nextInt();

        // Star Triangle
        System.out.println("Star Triangle:");
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                System.out.print("* ");
            }
            System.out.println();
        }

        // Number Triangle
        System.out.println("Number Triangle:");
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                System.out.print(j + " ");
            }
            System.out.println();
        }

        // Pyramid (centered stars)
        System.out.println("Pyramid:");
        for (int i = 1; i <= n; i++) {
            for (int s = 1; s <= n - i; s++) {
                System.out.print(" ");
            }
            for (int j = 1; j <= (2 * i - 1); j++) {
                System.out.print("*");
            }
            System.out.println();
        }
    }
}
```

### Sample Input/Output

```
Input: 4
Output (Star Triangle):
*
* *
* * *
* * * *

Output (Number Triangle):
1
1 2
1 2 3
1 2 3 4

Output (Pyramid):
   *
  ***
 *****
*******
```

### Explanation

Pattern printing relies on **nested loops**: the outer loop controls the row number, and the inner loop(s) control what's printed in that row. For the star triangle, row `i` prints `i` stars. For the pyramid, leading spaces decrease while the star count increases as `2i - 1`, creating the centered triangular shape.

### Time Complexity

O(n²) — since the inner loop's total work across all rows sums to roughly `1 + 2 + ... + n = n(n+1)/2`.

---

# Part C: Arrays

---

## C1. Sum of Array Elements

### Problem Statement

Compute the sum of all elements in an integer array.

### Algorithm

1. Initialize `sum = 0`.
2. Loop through each element, adding it to `sum`.
3. Print `sum`.

### Java Code

```java
public class SumOfArray {
    public static void main(String[] args) {
        int[] arr = {12, 45, 3, 67, 21};
        int sum = 0;

        for (int val : arr) {
            sum += val;
        }

        System.out.println("Sum = " + sum);
    }
}
```

### Sample Input/Output

```
Input: [12, 45, 3, 67, 21]
Output: Sum = 148
```

### Explanation

Each element is visited exactly once and accumulated into a running total using the enhanced for loop, since no index is needed.

### Time Complexity

O(n) — one pass through the array.

---

## C2. Average of Array Elements

### Problem Statement

Compute the average value of elements in an integer array.

### Algorithm

1. Compute the sum of all elements (as in C1).
2. Divide by the number of elements (`arr.length`), casting to `double` for decimal precision.

### Java Code

```java
public class AverageOfArray {
    public static void main(String[] args) {
        int[] arr = {12, 45, 3, 67, 21};
        int sum = 0;

        for (int val : arr) {
            sum += val;
        }

        double average = (double) sum / arr.length;
        System.out.println("Average = " + average);
    }
}
```

### Sample Input/Output

```
Input: [12, 45, 3, 67, 21]
Output: Average = 29.6
```

### Explanation

Casting `sum` to `double` before dividing prevents integer division truncation (e.g., `148 / 5` would give `29` instead of `29.6` if both operands were `int`).

### Time Complexity

O(n) — one pass to compute the sum, then O(1) for the division.

---

## C3. Maximum and Minimum in Array

### Problem Statement

Find the largest and smallest elements in an array.

### Algorithm

1. Initialize `max` and `min` to the first element.
2. Loop through the rest of the array, updating `max`/`min` when a larger/smaller value is found.

### Java Code

```java
public class MaxMinArray {
    public static void main(String[] args) {
        int[] arr = {12, 45, 3, 67, 21};
        int max = arr[0], min = arr[0];

        for (int val : arr) {
            if (val > max) max = val;
            if (val < min) min = val;
        }

        System.out.println("Max = " + max);
        System.out.println("Min = " + min);
    }
}
```

### Sample Input/Output

```
Input: [12, 45, 3, 67, 21]
Output:
Max = 67
Min = 3
```

### Explanation

By initializing both `max` and `min` to the first element, we avoid needing a special sentinel value (like `Integer.MIN_VALUE`/`MAX_VALUE`), and a single pass suffices to find both extremes.

### Time Complexity

O(n) — a single traversal finds both max and min simultaneously.

---

## C4. Reverse an Array

### Problem Statement

Reverse the order of elements in an array, in-place.

### Algorithm

1. Use two pointers: `left` at the start, `right` at the end.
2. Swap `arr[left]` and `arr[right]`.
3. Move `left` forward and `right` backward until they meet.

### Java Code

```java
import java.util.Arrays;

public class ReverseArray {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};
        int left = 0, right = arr.length - 1;

        while (left < right) {
            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            left++;
            right--;
        }

        System.out.println(Arrays.toString(arr));
    }
}
```

### Sample Input/Output

```
Input: [1, 2, 3, 4, 5]
Output: [5, 4, 3, 2, 1]
```

### Explanation

The **two-pointer technique** swaps elements from both ends, moving inward, until the pointers cross or meet — this reverses the array using only O(1) extra space (no second array needed).

### Time Complexity

O(n) time, O(1) extra space.

---

## C5. Count Even and Odd Numbers in Array

### Problem Statement

Count how many elements in an array are even and how many are odd.

### Algorithm

1. Initialize `evenCount = 0`, `oddCount = 0`.
2. Loop through the array; increment the appropriate counter based on `% 2`.

### Java Code

```java
public class CountEvenOdd {
    public static void main(String[] args) {
        int[] arr = {12, 45, 3, 67, 22, 8};
        int evenCount = 0, oddCount = 0;

        for (int val : arr) {
            if (val % 2 == 0) {
                evenCount++;
            } else {
                oddCount++;
            }
        }

        System.out.println("Even count = " + evenCount);
        System.out.println("Odd count = " + oddCount);
    }
}
```

### Sample Input/Output

```
Input: [12, 45, 3, 67, 22, 8]
Output:
Even count = 3
Odd count = 3
```

### Explanation

Each element is checked once against the modulus condition, and the corresponding counter is incremented — a simple classification pass over the array.

### Time Complexity

O(n) — one pass through the array.

---

## C6. Linear Search

### Problem Statement

Search for a target value in an array and return its index, or `-1` if not found.

### Algorithm

1. Loop through the array from index `0` to `length - 1`.
2. If the current element equals the target, return its index.
3. If the loop completes without a match, return `-1`.

### Java Code

```java
public class LinearSearch {
    public static int search(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                return i;
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] arr = {5, 3, 8, 1, 9};
        int target = 8;
        int result = search(arr, target);

        if (result != -1) {
            System.out.println("Found at index: " + result);
        } else {
            System.out.println("Not found");
        }
    }
}
```

### Sample Input/Output

```
Input: arr = [5, 3, 8, 1, 9], target = 8
Output: Found at index: 2

Input: arr = [5, 3, 8, 1, 9], target = 100
Output: Not found
```

### Explanation

Linear search examines each element sequentially, comparing it to the target. It works on both sorted and unsorted arrays, but in the worst case (element not present, or at the very end) it must check every element.

### Time Complexity

O(n) worst case, O(1) best case (target is the first element).

---

## C7. Matrix Addition

### Problem Statement

Add two matrices of the same dimensions, element-wise.

### Algorithm

1. Ensure both matrices have identical dimensions.
2. Loop through each row and column, adding corresponding elements.
3. Store the result in a new matrix.

### Java Code

```java
import java.util.Arrays;

public class MatrixAddition {
    public static void main(String[] args) {
        int[][] A = {{1, 2}, {3, 4}};
        int[][] B = {{5, 6}, {7, 8}};

        int rows = A.length, cols = A[0].length;
        int[][] result = new int[rows][cols];

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                result[i][j] = A[i][j] + B[i][j];
            }
        }

        for (int[] row : result) {
            System.out.println(Arrays.toString(row));
        }
    }
}
```

### Sample Input/Output

```
Input: A = [[1,2],[3,4]], B = [[5,6],[7,8]]
Output:
[6, 8]
[10, 12]
```

### Explanation

Matrix addition requires both matrices to have the same dimensions. Each element in the result matrix is the sum of the corresponding elements from `A` and `B`, computed using nested loops over rows and columns.

### Time Complexity

O(rows × cols) — every element is visited and processed exactly once.

---

## C8. Matrix Transpose

### Problem Statement

Compute the transpose of a matrix — flipping it over its diagonal so rows become columns.

### Algorithm

1. Create a new matrix with dimensions swapped (`cols × rows`).
2. Loop through the original matrix; assign `transpose[j][i] = original[i][j]`.

### Java Code

```java
import java.util.Arrays;

public class MatrixTranspose {
    public static void main(String[] args) {
        int[][] A = {
            {1, 2, 3},
            {4, 5, 6}
        };

        int rows = A.length, cols = A[0].length;
        int[][] transpose = new int[cols][rows];

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                transpose[j][i] = A[i][j];
            }
        }

        for (int[] row : transpose) {
            System.out.println(Arrays.toString(row));
        }
    }
}
```

### Sample Input/Output

```
Input: A = [[1,2,3],[4,5,6]]  (2x3)
Output:
[1, 4]
[2, 5]
[3, 6]   (3x2)
```

### Explanation

Each element at position `[i][j]` in the original matrix moves to position `[j][i]` in the transposed matrix. Since the original is `m x n`, the transpose must be allocated as `n x m` to accommodate the swapped dimensions.

### Time Complexity

O(rows × cols) — every element is visited and repositioned exactly once.

---

# Part D: Additional & Challenge Problems

_A few extra problems, including some tougher ones, to push your practice further._

---

## D1. GCD and LCM of Two Numbers

### Problem Statement

Find the Greatest Common Divisor (GCD) and Least Common Multiple (LCM) of two numbers.

### Algorithm

1. Use the **Euclidean algorithm**: `gcd(a, b) = gcd(b, a % b)`, until `b == 0`.
2. Compute `lcm(a, b) = (a * b) / gcd(a, b)`.

### Java Code

```java
public class GCDandLCM {
    public static int gcd(int a, int b) {
        while (b != 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }

    public static void main(String[] args) {
        int a = 36, b = 60;
        int gcdValue = gcd(a, b);
        int lcmValue = (a * b) / gcdValue;

        System.out.println("GCD = " + gcdValue);
        System.out.println("LCM = " + lcmValue);
    }
}
```

### Sample Input/Output

```
Input: a = 36, b = 60
Output:
GCD = 12
LCM = 180
```

### Explanation

The Euclidean algorithm repeatedly replaces `(a, b)` with `(b, a % b)` until `b` becomes 0 — at that point `a` holds the GCD. LCM is then derived using the identity `lcm(a,b) = (a*b) / gcd(a,b)`.

### Time Complexity

O(log(min(a, b))) — the Euclidean algorithm converges very quickly.

---

## D2. Check Perfect Number

### Problem Statement

A **perfect number** equals the sum of its proper divisors (excluding itself), e.g., `6 = 1 + 2 + 3`.

### Algorithm

1. Loop `i` from `1` to `n/2`.
2. If `n % i == 0`, add `i` to `sum`.
3. Compare `sum` with `n`.

### Java Code

```java
public class PerfectNumber {
    public static void main(String[] args) {
        int n = 28;
        int sum = 0;

        for (int i = 1; i <= n / 2; i++) {
            if (n % i == 0) {
                sum += i;
            }
        }

        System.out.println(n + (sum == n ? " is a Perfect Number" : " is Not a Perfect Number"));
    }
}
```

### Sample Input/Output

```
Input: 28
Output: 28 is a Perfect Number  (1+2+4+7+14 = 28)
```

### Explanation

Only divisors up to `n/2` need to be checked, since no proper divisor of `n` can exceed `n/2`. All such divisors are summed and compared to the original number.

### Time Complexity

O(n) — can be optimized to O(√n) by checking divisor pairs.

---

## D3. Binary Search (Sorted Array)

### Problem Statement

Search for a target value in a **sorted** array more efficiently than linear search.

### Algorithm

1. Set `low = 0`, `high = arr.length - 1`.
2. While `low <= high`: compute `mid`; if `arr[mid] == target`, return `mid`; if `arr[mid] < target`, search the right half; else search the left half.
3. Return `-1` if not found.

### Java Code

```java
public class BinarySearch {
    public static int search(int[] arr, int target) {
        int low = 0, high = arr.length - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] < target) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] arr = {1, 3, 5, 7, 9, 11, 13};
        int target = 9;
        int result = search(arr, target);

        System.out.println(result != -1 ? "Found at index: " + result : "Not found");
    }
}
```

### Sample Input/Output

```
Input: arr = [1,3,5,7,9,11,13], target = 9
Output: Found at index: 4
```

### Explanation

Binary search repeatedly halves the search space by comparing the target to the middle element, eliminating half the remaining elements each time. This requires the array to be **sorted**. `low + (high - low) / 2` is used instead of `(low + high) / 2` to avoid potential integer overflow.

### Time Complexity

O(log n) — a dramatic improvement over linear search's O(n) for large sorted datasets.

---

## D4. Bubble Sort

### Problem Statement

Sort an array in ascending order using the Bubble Sort algorithm.

### Algorithm

1. Repeatedly compare adjacent elements and swap them if out of order.
2. After each full pass, the largest unsorted element "bubbles" to its correct position at the end.
3. Repeat until no swaps are needed (array is sorted).

### Java Code

```java
import java.util.Arrays;

public class BubbleSort {
    public static void main(String[] args) {
        int[] arr = {64, 25, 12, 22, 11};
        int n = arr.length;

        for (int i = 0; i < n - 1; i++) {
            boolean swapped = false;
            for (int j = 0; j < n - 1 - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }
            if (!swapped) break; // already sorted, stop early
        }

        System.out.println(Arrays.toString(arr));
    }
}
```

### Sample Input/Output

```
Input: [64, 25, 12, 22, 11]
Output: [11, 12, 22, 25, 64]
```

### Explanation

The inner loop compares and swaps adjacent out-of-order pairs; the outer loop repeats this process, shrinking the unsorted portion by one each pass (since the largest element is guaranteed to reach its final position). The `swapped` flag allows early termination if the array becomes sorted before all passes complete.

### Time Complexity

O(n²) worst/average case; O(n) best case (already sorted, due to the early-exit optimization).

---

## D5. Kadane's Algorithm — Maximum Subarray Sum (Challenge)

### Problem Statement

Given an array of integers (which may include negative numbers), find the contiguous subarray with the largest sum.

### Algorithm

1. Initialize `maxSoFar = arr[0]`, `maxEndingHere = arr[0]`.
2. For each subsequent element, decide whether to extend the current subarray or start a new one: `maxEndingHere = max(arr[i], maxEndingHere + arr[i])`.
3. Update `maxSoFar` whenever `maxEndingHere` exceeds it.

### Java Code

```java
public class KadaneAlgorithm {
    public static void main(String[] args) {
        int[] arr = {-2, 1, -3, 4, -1, 2, 1, -5, 4};

        int maxSoFar = arr[0];
        int maxEndingHere = arr[0];

        for (int i = 1; i < arr.length; i++) {
            maxEndingHere = Math.max(arr[i], maxEndingHere + arr[i]);
            maxSoFar = Math.max(maxSoFar, maxEndingHere);
        }

        System.out.println("Maximum subarray sum = " + maxSoFar);
    }
}
```

### Sample Input/Output

```
Input: [-2, 1, -3, 4, -1, 2, 1, -5, 4]
Output: Maximum subarray sum = 6   (subarray: [4, -1, 2, 1])
```

### Explanation

This is a classic **dynamic programming** problem. At each position, we decide whether extending the previous subarray is more beneficial than starting fresh from the current element. This greedy/DP hybrid approach avoids the O(n²) brute-force method of checking every possible subarray.

### Time Complexity

O(n) — a significant improvement over the brute-force O(n²) or O(n³) approaches.

---

## D6. Matrix Multiplication (Challenge)

### Problem Statement

Multiply two matrices `A` (m×n) and `B` (n×p) to produce a result matrix (m×p).

### Algorithm

1. Verify `A`'s columns equal `B`'s rows.
2. For each cell `result[i][j]`, compute the dot product of row `i` of `A` and column `j` of `B`.

### Java Code

```java
import java.util.Arrays;

public class MatrixMultiplication {
    public static void main(String[] args) {
        int[][] A = {{1, 2, 3}, {4, 5, 6}};       // 2x3
        int[][] B = {{7, 8}, {9, 10}, {11, 12}};  // 3x2

        int rowsA = A.length, colsA = A[0].length, colsB = B[0].length;
        int[][] result = new int[rowsA][colsB];

        for (int i = 0; i < rowsA; i++) {
            for (int j = 0; j < colsB; j++) {
                int sum = 0;
                for (int k = 0; k < colsA; k++) {
                    sum += A[i][k] * B[k][j];
                }
                result[i][j] = sum;
            }
        }

        for (int[] row : result) {
            System.out.println(Arrays.toString(row));
        }
    }
}
```

### Sample Input/Output

```
Input: A (2x3) = [[1,2,3],[4,5,6]], B (3x2) = [[7,8],[9,10],[11,12]]
Output:
[58, 64]
[139, 154]
```

### Explanation

Each element of the result matrix is a dot product: `result[i][j] = sum(A[i][k] * B[k][j])` across the shared dimension `k`. This requires three nested loops — one for output rows, one for output columns, and one for the shared dimension.

### Time Complexity

O(m × n × p) — commonly simplified to O(n³) for square matrices.

---

## D7. Sieve of Eratosthenes — All Primes up to N (Challenge)

### Problem Statement

Find all prime numbers from `2` up to `n` efficiently.

### Algorithm

1. Create a boolean array `isComposite[0..n]`, initially all `false`.
2. For each `i` from `2` to `√n`, if `isComposite[i]` is `false`, mark all multiples of `i` (starting from `i*i`) as composite.
3. All indices still marked `false` (except 0 and 1) are prime.

### Java Code

```java
public class SieveOfEratosthenes {
    public static void main(String[] args) {
        int n = 50;
        boolean[] isComposite = new boolean[n + 1];

        for (int i = 2; (long) i * i <= n; i++) {
            if (!isComposite[i]) {
                for (int j = i * i; j <= n; j += i) {
                    isComposite[j] = true;
                }
            }
        }

        System.out.print("Primes up to " + n + ": ");
        for (int i = 2; i <= n; i++) {
            if (!isComposite[i]) {
                System.out.print(i + " ");
            }
        }
    }
}
```

### Sample Input/Output

```
Input: n = 50
Output: Primes up to 50: 2 3 5 7 11 13 17 19 23 29 31 37 41 43 47
```

### Explanation

Instead of checking each number individually for primality (which would take O(n√n) total), the Sieve marks off multiples of each prime starting from `i*i` (smaller multiples would already be marked by smaller primes). This is dramatically faster for generating **all** primes up to a limit.

### Time Complexity

O(n log log n) — much faster than checking each number individually.

---

## D8. Rotate an Array by K Positions (Challenge)

### Problem Statement

Rotate an array to the left by `k` positions, in-place, using O(1) extra space.

### Algorithm (Reversal Technique)

1. Reverse the first `k` elements.
2. Reverse the remaining `n - k` elements.
3. Reverse the entire array.

### Java Code

```java
import java.util.Arrays;

public class RotateArray {
    public static void reverse(int[] arr, int start, int end) {
        while (start < end) {
            int temp = arr[start];
            arr[start] = arr[end];
            arr[end] = temp;
            start++;
            end--;
        }
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5, 6, 7};
        int k = 2;
        k = k % arr.length; // handle k > array length

        reverse(arr, 0, k - 1);
        reverse(arr, k, arr.length - 1);
        reverse(arr, 0, arr.length - 1);

        System.out.println(Arrays.toString(arr));
    }
}
```

### Sample Input/Output

```
Input: arr = [1,2,3,4,5,6,7], k = 2
Output: [3, 4, 5, 6, 7, 1, 2]
```

### Explanation

This clever technique performs a **left rotation** using only reversals: reversing the first `k` elements and the remaining elements separately, then reversing the whole array, produces the correctly rotated result — all without any extra array.

### Time Complexity

O(n) time, O(1) extra space — each element is touched a constant number of times across the three reversal passes.

---

## D9. Check if a String is a Palindrome (Bonus)

### Problem Statement

Check whether a given string reads the same forwards and backwards, ignoring case.

### Algorithm

1. Convert the string to lowercase.
2. Use two pointers from both ends, comparing characters and moving inward.
3. If any mismatch is found, it's not a palindrome.

### Java Code

```java
public class StringPalindrome {
    public static void main(String[] args) {
        String str = "Madam";
        String s = str.toLowerCase();
        boolean isPalindrome = true;

        int left = 0, right = s.length() - 1;
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                isPalindrome = false;
                break;
            }
            left++;
            right--;
        }

        System.out.println(str + " is " + (isPalindrome ? "a Palindrome" : "Not a Palindrome"));
    }
}
```

### Sample Input/Output

```
Input: "Madam"
Output: Madam is a Palindrome

Input: "Hello"
Output: Hello is Not a Palindrome
```

### Explanation

Case is normalized first so "Madam" and "madam" are treated identically. The two-pointer comparison checks characters symmetrically from both ends, stopping early on the first mismatch.

### Time Complexity

O(n) — at most one full pass through half the string.

---

## D10. Find the Missing Number in an Array (Challenge)

### Problem Statement

Given an array containing `n-1` distinct integers from `1` to `n`, find the missing number.

### Algorithm

1. Compute the expected sum of `1` to `n` using the formula `n*(n+1)/2`.
2. Compute the actual sum of the array's elements.
3. The missing number is the difference between the expected and actual sums.

### Java Code

```java
public class MissingNumber {
    public static void main(String[] args) {
        int[] arr = {1, 2, 4, 5, 6}; // missing 3, n = 6
        int n = arr.length + 1;

        int expectedSum = n * (n + 1) / 2;
        int actualSum = 0;
        for (int val : arr) {
            actualSum += val;
        }

        int missing = expectedSum - actualSum;
        System.out.println("Missing number = " + missing);
    }
}
```

### Sample Input/Output

```
Input: [1, 2, 4, 5, 6]
Output: Missing number = 3
```

### Explanation

This is a classic O(n)-time, O(1)-space trick: since we know the sum of all numbers from `1` to `n` should be `n(n+1)/2`, subtracting the actual array sum from this expected sum directly yields the missing value — no sorting or searching required.

### Time Complexity

O(n) — a single pass to sum the array elements.

---

# Summary Table

| #   | Problem               | Category        | Time Complexity  |
| --- | --------------------- | --------------- | ---------------- |
| A1  | Even/Odd Check        | Decision Making | O(1)             |
| A2  | Largest of Three      | Decision Making | O(1)             |
| A3  | Grade Calculator      | Decision Making | O(1)             |
| A4  | Leap Year Checker     | Decision Making | O(1)             |
| A5  | Switch Calculator     | Decision Making | O(1)             |
| B1  | Print Numbers         | Loops           | O(n)             |
| B2  | Sum of First N        | Loops           | O(n)             |
| B3  | Factorial             | Loops           | O(n)             |
| B4  | Fibonacci Series      | Loops           | O(n)             |
| B5  | Prime Check           | Loops           | O(√n)            |
| B6  | Armstrong Number      | Loops           | O(log n)         |
| B7  | Palindrome (Number)   | Loops           | O(log n)         |
| B8  | Reverse a Number      | Loops           | O(log n)         |
| B9  | Multiplication Table  | Loops           | O(1)             |
| B10 | Pattern Printing      | Loops           | O(n²)            |
| C1  | Sum of Array          | Arrays          | O(n)             |
| C2  | Average of Array      | Arrays          | O(n)             |
| C3  | Max/Min in Array      | Arrays          | O(n)             |
| C4  | Reverse Array         | Arrays          | O(n)             |
| C5  | Count Even/Odd        | Arrays          | O(n)             |
| C6  | Linear Search         | Arrays          | O(n)             |
| C7  | Matrix Addition       | Arrays          | O(rows×cols)     |
| C8  | Matrix Transpose      | Arrays          | O(rows×cols)     |
| D1  | GCD & LCM             | Extra           | O(log(min(a,b))) |
| D2  | Perfect Number        | Extra           | O(n)             |
| D3  | Binary Search         | Extra           | O(log n)         |
| D4  | Bubble Sort           | Extra           | O(n²)            |
| D5  | Kadane's Algorithm    | Challenge       | O(n)             |
| D6  | Matrix Multiplication | Challenge       | O(n³)            |
| D7  | Sieve of Eratosthenes | Challenge       | O(n log log n)   |
| D8  | Rotate Array by K     | Challenge       | O(n)             |
| D9  | String Palindrome     | Bonus           | O(n)             |
| D10 | Missing Number        | Challenge       | O(n)             |

---

_End of Chapter 3 — Practice Problems (Java)_
