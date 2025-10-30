Recursion is a technique where a function calls itself to solve a problem. It's often used when a problem can be broken down into smaller, similar subproblems.
- a function continues to call itself until it reaches a **base case**—a condition that stops the recursion.
- Without a base case, the function would call itself infinitely, leading to a stack overflow error.
####  A recursive function generally has two main parts:

- **Base Case:** The condition that terminates the recursion.
- **Recursive Step:** The part where the function calls itself with a [simplified or smaller version] of the problem, [moving closer to the base case.]

![[Pasted image 20251018174141.png]]

#Example: Calculating Factorial
```java
static int factorial(n)
{ if 
  n == 0:                        // Base Case return 1 
else:                           // Recursive Step 
  return n * factorial(n - 1)
}
```

### How Function Calls Work Internally

When a function is called in a running program, the execution environment (like the operating system and the programming language runtime) uses a data structure called the **Call Stack** (or just "the stack") to manage the flow of control.

*The Call Stack
The Call Stack is a region of memory that operates using a **LIFO** (Last-In, First-Out) principle

- **Push:** When a function is called, a new record, called a **Stack Frame** (or **Activation Record** ), is **pushed** onto the top of the stack.
    
- **Pop:** When a function completes its execution (either by returning a value or reaching its end), its Stack Frame is **popped** (removed) from the top of the stack, [and execution returns to the function below it.]

*What's in a Stack Frame?
A Stack Frame holds all the [necessary information] for a function call to execute correctly and return to the caller. This typically includes:

1. **Local Variables:** The variables declared inside that specific function call.
2. **Parameters/Arguments:** The values passed to the function.
3. **Return Address:** A pointer to the instruction in the calling function where execution should resume after the current function finishes.
4. **Other bookkeeping information** (e.g., saved register values).

#Example 
String Reverse 
![[Pasted image 20251018180231.png]]
![[Pasted image 20251018180322.png]]

#Example
Is Palindrome 
![[Pasted image 20251018181909.png]]

![[Pasted image 20251018181936.png]]
### Error
This process of popping the stack frames and returning values is called **unwinding the stack**. Because each recursive call adds a new frame, very deep recursion can exhaust the limited memory space of the Call Stack, leading to a **Stack Overflow Error**.


## Recursion With Numbers
#Example
*Decimal to Binary 

 #addthenumbers
![[Pasted image 20251018202929.png]]
![[Pasted image 20251018203745.png]] 

#Example 
![[Pasted image 20251018205802.png]]
![[Pasted image 20251018205723.png]]

