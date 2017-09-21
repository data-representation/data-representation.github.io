# Problem set: Go fundamentals
These are a few problems from around the internet to learn the fundamentals of [the Go programming language](https://golang.org/).
Create a repository on [GitHub](https://github.com/) to store your answers, complete with a README.


## 1. Kon'nichiwa, Sekai!

Source:
[https://tour.golang.org/welcome/1](https://tour.golang.org/welcome/1)

Write a program that prints "Hello, world!" in Japanese (using Japanese characters) to the screen.


## 2. Current time

Source: [https://tour.golang.org/welcome/4](https://tour.golang.org/welcome/4)

Write a program that prints the current time and date to the console.


## 3. FizzBuzz

Source: [http://wiki.c2.com/?FizzBuzzTest](http://wiki.c2.com/?FizzBuzzTest)

Write a program that prints the numbers from 1 to 100, except for the following conditions.
For multiples of three print "Fizz" instead of the number, and for the multiples of five print "Buzz".
For numbers which are multiples of both three and five print "FizzBuzz".


## 4. Factorial digit sum

Write a function that calculates the sum of the digits of the factorial of a number.
n! means n x (n − 1)  ... x 3 x 2 x 1.
For example, 10! = 10 x 9 x ... x 3 x 2 x 1 = 3628800, and the sum of the digits in the number 10! is 3 + 6 + 2 + 8 + 8 + 0 + 0 = 27.
Find the sum of the digits in the number 100!


## 5. Guessing game

Source: [https://adriann.github.io/programming_problems.html](https://adriann.github.io/programming_problems.html)

Write a guessing game where the user must guess a secret number. After every guess the program tells the user whether their number was too large or too small. At the end the number of tries needed should be printed. It counts only as one try if they input the same number multiple times consecutively.


## 6. Largest and smallest in list

Source: [https://adriann.github.io/programming_problems.html](https://adriann.github.io/programming_problems.html)

Write a function that returns the largest and smallest elements in a list.


## 7. Palindrome test

Source: [https://adriann.github.io/programming_problems.html](https://adriann.github.io/programming_problems.html)

Write a function that tests whether a string is a palindrome.


## 8. Merge list and sort

Source: [https://adriann.github.io/programming_problems.html](https://adriann.github.io/programming_problems.html)

Write a function that merges two sorted lists into a new sorted list. [1,4,6],[2,3,5] → [1,2,3,4,5,6].


## 9. Newton's method for square roots

Source: [https://tour.golang.org/flowcontrol/8](https://tour.golang.org/flowcontrol/8)

Implement the square root function using Newton's method.
In this case, Newton's method is to approximate sqrt(x) by picking a starting point z and then repeating:

```go
z_next = z - ((z*z - x) / (2 * z))
```

To begin with, just repeat that calculation 10 times and see how close you get to the answer for various values (1, 2, 3, ...).
Next, change the loop condition to stop once the value has stopped changing (or only changes by a very small delta).
How close are you to the [math.Sqrt](https://golang.org/pkg/math/#Sqrt) value?

Hint: to declare and initialize a floating point value, give it floating point syntax or use a conversion:

```go
z := float64(1)
z := 1.0
```


## 10. Reverse string
Write a function to reverse a string in Go.
