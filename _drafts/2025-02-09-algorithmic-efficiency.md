---
layout: post
title: "Algorithmic Efficiency"
date: 2025-02-09
categories: ['software engineering']
tags: ['algorithms', 'efficiency', 'big-o notation']
---

Algorithms are often created to solve specific problems. How efficiently an algorithm gets to the solution is an important consideration. The two measures of efficiency are execution time (time complexity) and memory (space complexity). Typically the biggest influence on these two measures is the input size, `n`. Input size may refer to the _count_ of elements (sorting 10,000 numbers takes longer than 10) or the _magnitude_ of the value (calculating the 100th Fibonacci number takes longer than the 10th).

Big-O notation is a general way of measuring an algorithm's efficiency or complexity with respect to `n`. For example, Big-O notation can be used to compare the complexity of different sorting algorithms:

| Algorithm | Time Complexity (Avg) | Space Complexity |
| :-------- | :-------------------: | :--------------: |
| [Heapsort](https://en.wikipedia.org/wiki/Heapsort) | O(n log n) | O(1) |
| [Merge sort](https://en.wikipedia.org/wiki/Merge_sort) | O(n log n) | O(n) |
| [Bogosort](https://en.wikipedia.org/wiki/Bogosort) | O(n * n!)  | O(1) |

This table shows how these algorithms will perform, providing a guide for the suitability of the algorithm for the specific problem. Bogosort, for instance, might work for sorting five numbers, but is impractical for even 20 numbers due to its time complexity.

Ideally, an algorithm will have a closed-form or direct solution, that is, a time complexity of O(1). This means the algorithm’s execution time remains constant, regardless of the input size. However, for many problems, deriving a closed-form solution is not straight-forward or may not even be feasible. For example, the complexity of a sorting algorithm depends on the number of items to be sorted.

But when possible, achieving a direct solution offers the following key advantages:
- **Lower resource requirements**: Typically less processing and memory is required as fewer intermediate steps are needed.
- **Reliability**: More reliable in real-time or mission-critical systems where performance and speed are paramount.

Though the following caveats should be considered:
- **Implementation Simplicity**: A direct solution may be more difficult to read, understand and explain due to it being unintuitive to comprehend. This may also impact the ability to debug and maintain the algorithm.
- **Execution time overhead**: For smaller input sizes, a direct solution's _constant_ execution time might take longer than algorithms with higher complexities. Additionally, even with O(1) complexity, execution time can vary, such as from factors like input-related collisions in a hash table.
- **Limited flexibility**: A direct solution may be less adaptable to other problems compared to more generalised algorithms.
- **Numerical precision**: The computing system limitations need to be considered for the algorithm, such as rounding errors from the mathematical imprecision of floating-point arithmetic.

Despite these caveats, when a direct solution is available, it often provides significant benefits over higher-complexity algorithms, particularly in terms of performance.

Overall, the suitability of an algorithm in regards to the problem being solved is the trade-off between the key advantages and the caveats. This is demonstrated by the following algorithms for calculating the n-th Fibonacci number (ordered from the highest time complexity to the lowest).


## Naïve Recursive Algorithm

Time Complexity: **O(2^n)** \
Space Complexity: **O(n)**

The naïve recursive algorithm has exponential time complexity due to redundant calculations.

```c#
public int FibonacciRecursive(int n)
{
   if (n <= 1)
   {
      return n;
   }
      
   return FibonacciRecursive(n - 1) +
          FibonacciRecursive(n - 2);
}
```


## Recursive with Memoization

Time Complexity: **O(n)** \
Space Complexity: **O(n)**

This approach uses a dictionary to store the results of sub-problems and reuses them, making it more efficient.

```C#
private Dictionary<int, int> memo = [];

public int FibonacciMemo(int n)
{
   if (memo.ContainsKey(n))
   {
      return memo[n];
   }
      
   if (n <= 1)
   {
      return n;
   }

   memo[n] = FibonacciMemo(n - 1) +
             FibonacciMemo(n - 2);

   return memo[n];
}
```


## Dynamic Programming

Time Complexity: **O(n)** \
Space Complexity: **O(n)**

This approach uses an array to store intermediate Fibonacci results.

```C#
public int FibonacciDynamicProgramming(int n)
{
   if (n <= 1)
   {
      return n;
   }

   var dp = new int[n + 1];
   dp[1] = 1;

   for (int i = 2; i <= n; i++)
   {
      dp[i] = dp[i - 1] + dp[i - 2];
   }

   return dp[n];
}
```


## Iterative Approach

Time Complexity: **O(n)** \
Space Complexity: **O(1)**

The iterative approach computes the Fibonacci number using a loop. This is the closest implementation of how the Fibonacci sequence is generally thought about, making it more intuitive to comprehend.

```C#
public int FibonacciIterative(int n)
{
   var a = 0;
   var b = 1;

   for (var i = 0; i < n; i++)
   {
       var c = a + b;
       a = b;
       b = c;
   }

   return a;
}
```


## Matrix Exponentiation

Time Complexity: **O(log n)** \
Space Complexity: **O(1)**

This method uses matrix exponentiation to compute the Fibonacci number in logarithmic time. More performant, but requires knowledge of matrix math to intuitively follow.

```C#
public int FibonacciMatrix(int n)
{
   if (n == 0)
   {
      return 0;
   }

   var power = n - 1;
   var baseMatrix = new int[,]
   {
      { 1, 1 },
      { 1, 0 }
   };

   var result = new int[,]
   {
      { 1, 0 },
      { 0, 1 }
   }; // Initially the identity matrix

   while (power > 0)
   {
      if (power % 2 == 1)
      {
         result = MatrixMultiply(result, baseMatrix);
      }

      baseMatrix = MatrixMultiply(baseMatrix, baseMatrix);
      power /= 2;
   }

   return result[0, 0];  // Fibonacci number is at position [0, 0]
}

public int[,] MatrixMultiply(int[,] A, int[,] B)
{
   return new int[,]
   {
      { 
         A[0, 0] * B[0, 0] + A[0, 1] * B[1, 0],
         A[0, 0] * B[0, 1] + A[0, 1] * B[1, 1]
      },
      { 
         A[1, 0] * B[0, 0] + A[1, 1] * B[1, 0],
         A[1, 0] * B[0, 1] + A[1, 1] * B[1, 1]
      }
   };
}
```


## Direct Formula - O(1)

Time Complexity: **O(1)** \
Space Complexity: **O(1)**

Binet's formula is a direct solution for Fibonacci. However, its accuracy has limits when `n` is large. This implementation encounters integer overflow at `n = 47`, but even if `long` were used, floating-point precision limitations would occur at `n = 71`.

While this is a concise algorithm, its mathematical derivation is abstract and not obvious from the code.

```C#
public int FibonacciBinet(int n)
{
   var sqrt5 = Math.Sqrt(5);
   var phi = (1 + sqrt5) / 2;

   return (int)Math.Round((Math.Pow(phi, n) - Math.Pow(-phi, -n)) / sqrt5);
}
```
