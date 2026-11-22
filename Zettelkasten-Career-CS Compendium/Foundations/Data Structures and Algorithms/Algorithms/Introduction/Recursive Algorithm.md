---
tags:
  - Algorithm
---
The process in which a function calls itself directly or indirectly is called recursion and the corresponding function is called a recursive function. Using a recursive algorithm, certain problems can be solved quite easily. Examples of such problems are Towers of Hanoi (TOH), Inorder/Preorder/Postorder Tree Traversals, DFS of Graph, etc

A recursive function solves a particular problem by calling a copy of itself and solving smaller subproblems of the original problems. Many more recursive calls can be generated as and when required. It is essential to know that we should provide a certain case in order to terminate this recursion process. So we can say that every time the function calls itself with a simpler version of the original problem.

# Need of recursion
Recursion is an amazing technique with the help of which we can reduce the length of our code and make it easier to read and write. It has certain advantages over the iteration technique which will be discussed later. A task that can be defined with its similar subtask, recursion is one of the best solutions for it. For example; The Factorial of a number.

# Properties of recursion
- Performing the same operations multiple times with different inputs.
- In every step, we try smaller inputs to make the problem smaller.
- Base condition is needed to stop the recursion otherwise infinite loop will occur.
# How are recursion functions stored in memory
Recursion uses more memory, because the recursive function adds to the stack with each recursive call, and keeps the values there until the call is finished. The recursive function uses LIFO (LAST IN FIRST OUT) Structure just like the stack data structure. https://www.geeksforgeeks.org/stack-data-structure
# Base case
In the recursive program, the solution to the base case is provided and the solution to the bigger problem is expressed in terms of smaller problems. 
````
int fact(int n)
{
    if (n < = 1) // base case
        return 1;
    else    
        return n*fact(n-1);    
}
````
In the above example, the base case for `n < = 1` is defined and the larger value of a number can be solved by converting to a smaller one till the base case is reached.

# Why Stack Overflow error occurs in recursion? 
If the base case is not reached or not defined, then the stack overflow problem may arise. Let us take an example to understand this.
# What is the difference between direct and indirect recursion? 
A function fun is called direct recursive if it calls the same function fun. A function fun is called indirect recursive if it calls another function say fun_new and fun_new calls fun directly or indirectly. The difference between direct and indirect recursion has been illustrated in Table 1. 
````
// An example of direct recursion
void directRecFun()
{
    // Some code....

    directRecFun();

    // Some code...
}

// An example of indirect recursion
void indirectRecFun1()
{
    // Some code...

    indirectRecFun2();

    // Some code...
}
void indirectRecFun2()
{
    // Some code...

    indirectRecFun1();

    // Some code...
}
````
# Tailed and non-tailed recursion
A recursive function is tail recursive when a recursive call is the last thing executed by the function. Please refer tail recursion article for details. 

# Complexity
Time Complexity: `O(1)`
Auxiliary Space: `O(1)`
````
// A Java program to demonstrate working of 
// recursion 
class GFG { 
	static void printFun(int test) 
	{ 
		if (test < 1) 
			return; 
		else { 
			System.out.printf("%d ", test); 
			printFun(test - 1); // statement 2 
			System.out.printf("%d ", test); 
			return; 
		} 
	} 

	// Driver Code 
	public static void main(String[] args) 
	{ 
		int test = 3; 
		printFun(test); 
	} 
} 

// This code is contributed by 
// Smitha Dinesh Semwal
````

- When `printFun(3)` is called from `main()`, memory is allocated to `printFun(3)` and a local variable test is initialized to 3 and statement 1 to 4 are pushed on the stack as shown in below diagram. 
- It first prints `3`. 
- In statement 2, `printFun(2)` is called and memory is allocated to `printFun(2)` and a local variable test is initialized to 2 and statement 1 to 4 are pushed into the stack. 
- Similarly, `printFun(2)` calls `printFun(1)` and `printFun(1)` calls `printFun(0)`. 
- `printFun(0)` goes to if statement and it return to `printFun(1)`. 
- The remaining statements of `printFun(1)` are executed and it returns to `printFun(2)` and so on. 
- In the output, values from 3 to 1 are printed and then 1 to 3 are printed. The memory stack has been shown in below diagram.

# Recursion vs Iteration
Recursion is a programming technique where a function calls itself in order to solve a problem by breaking it down into smaller, similar subproblems. A recursive function usually includes a base case that stops the recursion once a certain condition is met. Recursion can make code look cleaner and more intuitive, especially for problems that have a natural hierarchical structure, like tree traversal or calculating factorials. However, recursion can be less efficient because each function call uses stack memory, which can lead to a stack overflow if the recursion is too deep.

Iteration, on the other hand, solves problems by repeating a set of instructions using loops, such as `for` or `while` loops. The loop continues until a defined condition is no longer satisfied. Iterative solutions are often more memory-efficient because they do not add new frames to the call stack. They tend to perform better in terms of speed and resource usage, although the logic can sometimes be more complex and less intuitive for problems that naturally fit a recursive approach.

# Overhead
The difference in overhead in recursion vs iteration is an important consideration in software development. Overhead means any additional computational methods or resources required to attain a particular functionality. In terms of recursive and iterative functions, it relates to the workload the system must do to execute and manage the code.

All recursive calls require the overhead to create a new function on the call stack. As the recursion grows, the call stack expands, potentially using significant memory. Besides, this cycled function calling increases the code’s time complexity, making it feeble or less efficient.

However, iterative functions avoid using overheads. Instead, this approach uses loop control structures to control flow, manage variables, and repeat code blocks. Since it lacks stack management and recursive calls, iteration is time-efficient and utilizes less memory. Iteration also plays a vital role in helping you achieve your product development workflow faster.

# Video
https://youtu.be/k-7jJP7QFEM?si=2L389roM40ka9lj2

