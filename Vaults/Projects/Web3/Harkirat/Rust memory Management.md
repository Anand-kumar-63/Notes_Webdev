-  There are three ways to manage the memory- garbage collector langs , memory managment langs , and the rust way
![[Pasted image 20251209181906.png]]
- How rust memory management works
![[Pasted image 20251209182029.png]]


## [four most important jargons]
  # Mutability  # ownership  # Borrowing  # References

## Mutability in rust
- All the variables in rust are by default immutable![[Pasted image 20251209182505.png]]
## Stacks Vs Heap 

![[Pasted image 20251209193833.png]]

variables that changes their values at the run time like vectors(dynamic array) and strings they are not stored on the stack , there pointers are stored on the stack while they are stored on the heap  , they are given memory space on the heap 
![[Pasted image 20251209195656.png]]
![[Pasted image 20251209195903.png]]	 

- How the variables are actually pushed on to the stack and how the stack frame is created...?? 
- there is a stack frame for the main function and that gets pushed onto the stack
![[Pasted image 20251209200350.png]]
A function calling another function - 
![[Pasted image 20251209200434.png]]

when the execution of first function gets done then the that frame pops out from the stack 
and pointer points back to the original main function and stack willl look like this
![[Pasted image 20251209200708.png]]

## But what if you have data that changes space at run time?? 
It gets stored on the heap 
![[Pasted image 20251209200854.png]]
- See what happens is that whenver there is something that grows and shrinks at run time it is stored in heap in a contigeous memory system
- Rust compiler ask the OS allocated some contigeous memory space in the Heap of ram and the pointer to the first index is stored in the stack memory and when it grows the rust comipler ask more space from the os in the hap memory 
#example 
![[Pasted image 20251209201410.png]]
![[Pasted image 20251209201421.png]]
when heap function get called after the execution of the Stack_fn() so a stack frame  gets created and then three string fames stores the capacity , length , pointer get stored and heap gets created to store the string value
#function_string_combine
![[Pasted image 20251209201602.png]]
#functino_string_update
![[Pasted image 20251209202204.png]]
see when you have a data variable that that grows at runtime so when you store it in the heap memo its not necessary that you will be getting the contegeous memory when the variable will grow later it might be possible that some other process is using that space just after the string memory so what rust compiler do is to reallocate the memory  of the string in the heap to get some extra memory space in contegeous manner.
this will change the value of the pointer and when the length is changing the capacity will also get changed....
you can check it using the code
![[Pasted image 20251209215258.png]]
## Ownership 

#readmore Ref - [https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html)
![[Pasted image 20251209222351.png]]
![[Pasted image 20251209231330.png]]

### stack variables 
[](https://projects.100xdevs.com/tracks/rust-bootcamp/Rust-Bootcamp-13#790e3a48ea3146a0babc11e0541f3421 "Example #1 - Passing stack Variables inside functions")Example #1 - Passing stack Variables inside functions
```javascript
fn main() {
		let x = 1; // crated on stack
		let y = 3; // created on stack
    println!("{}", sum(x, y));
    println!("Hello, world!");
}

fn sum(a: i32, b: i32) -> i32 {
    let c = a + b;
    return c;
}
```
This might sound trivial since if the function is popped of the stack, all variables go away with it, but check the next example
### 
[](https://projects.100xdevs.com/tracks/rust-bootcamp/Rust-Bootcamp-13#1d1e3895b39144b6b9b35b3e2275f4a3 "Example #2 - Scoping variables in the same fn")Example #2 - Scoping variables in the same fn
```javascript
fn main() {
    let x = 1; // crated on stack
    {
        let y = 3; // created on stack
    }

    println!("{}", y); // throws error
}
```
### Heap variables
[] (https://projects.100xdevs.com/tracks/rust-bootcamp/Rust-Bootcamp-13#fd806c68661f4eb2b44d098498b7f493 "Heap variables")

Heap variables are like Rihana. They always want to have a `single` owner, and if their owner goes out of scope, they get deallocated.
Any time the owner of a `heap variable` goes out of scope, the value is de-allocated from the heap.

[](https://projects.100xdevs.com/tracks/rust-bootcamp/Rust-Bootcamp-13#4aad7e00139b47cf801879d5758bb013 "Example #1 - Passing strings (heap variables) to functions as args")Example #1 - Passing strings (heap variables) to functions as args
```javascript
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;
    println!("{}", s1); // This line would cause a compile error because ownership has been moved.
}
```

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F165f9686-4e14-4160-bde4-08c3340c14e3%2Ftrpl04-04.svg?table=block&id=d5d261e4-d8f5-48fb-b92f-fe2bbcfc2306&cache=v2)

Another example of the same thing -

```javascript
fn main() {
    let my_string = String::from("hello");
    takes_ownership(my_string);
    println!("{}", my_string); // This line would cause a compile error because ownership has been moved.
}

fn takes_ownership(some_string: String) {
    println!("{}", some_string); // `some_string` now owns the data.
}
```
At any time, each value can have a `single` owner. This is to avoid memory issues like
1. Double free error.
2. Dangling pointers.
### fix
[](https://projects.100xdevs.com/tracks/rust-bootcamp/Rust-Bootcamp-13#f4459731eaf24471906800f362fb87d7 "Fix?")Fix?
Clone the string
```javascript
fn main() {
    let s1 = String::from("hello");
    let s2 = s1.clone();
    println!("{}", s1); // Compiles now
}
```

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F2eace7ca-252a-4eea-96fc-78deef6b586b%2FScreenshot_2024-04-26_at_9.08.01_AM.png?table=block&id=036c4833-6e1a-4c72-a64c-dc80124fd1c7&cache=v2)

But what if you want to pass the same string over to the function? You don’t want to clone it, and you want to return back ownership to the original function?

You can either do the following -
```javascript
fn main() {
    let s1 = String::from("hello");
    let s2 = takes_ownership(s1);
    println!("{}", s2);
}

fn takes_ownership(some_string: String) -> String {
    println!("{}", some_string); 
    return some_string; // return the string ownership back to the original main fn
}
```

You can also do this
Is there a better way to pass strings (or generally heap related data structures) to a function without passing over the ownership? Yes - References


##  **Borrowing and references**

[](https://projects.100xdevs.com/tracks/rust-bootcamp/Rust-Bootcamp-14#355a8a746ce747bd89c00e5d7a3b23fb "Rihana upgrades")Rihana upgrades
Rihana now says I’d like to be borrowed from time to time. I will still have a `single owner`, but I can still be borrowed by other variables temporarily. What rules do you think should apply to her?
1. She can be borrowed by multiple people that she’s friends with but does no hanky panky
2. If she does want to do hanky panky, she can only have `1` borrower that she does it with. She cant simultaneously be with other borrowers (even with no hanky panky)

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Fd93c3c3f-023c-471b-9034-3560a51c6a9d%2FY1SjVJFtS5K574nvXTHigg.jpeg?table=block&id=92b623f4-3970-4c52-a62b-5802230c36fc&cache=v2)

### References
[](https://projects.100xdevs.com/tracks/rust-bootcamp/Rust-Bootcamp-14#cd9f5b1d46af432b92412bf3714dd62b "References")References
References mean giving the address of a string rather than the ownership of the string over to a function

For example
```javascript
fn main() {
    let s1 = String::from("Hello");
    let s2 = &s1;

    println!("{}", s2);
    println!("{}", s1);    // This is valid, The first pointer wasn't invalidated
}
```

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F01536509-0350-4ee4-ba6e-fcb838cc32ae%2FScreenshot_2024-04-26_at_9.27.08_AM.png?table=block&id=d2216029-bfeb-41f7-81b4-a04762520203&cache=v2)

### Borrowing 
[](https://projects.100xdevs.com/tracks/rust-bootcamp/Rust-Bootcamp-14#a41a788e16254ded909a8bcdd23207ab "Borrowing")**Borrowing**
You can transferring ownership of variables to fns. By passing a reference to the string to the function `take_ownership`, the ownership of the string remains with the original variable, in the `main`function. This allows you to use `my_string`again after the function call.
```javascript
fn main() {
    let my_string = String::from("Hello, Rust!");
    takes_ownership(&my_string);  // Pass a reference to my_string
    println!("{}", my_string);    // This is valid because ownership was not transferred
}

fn takes_ownership(some_string: &String) {
    println!("{}", some_string);  // some_string is borrowed and not moved
}
```

### Mutable references
[](https://projects.100xdevs.com/tracks/rust-bootcamp/Rust-Bootcamp-14#ac35c71fd9354162aba80cc1f9d66b43 "Mutable references")Mutable references
What if you want a function to `update` the value of a variable?
```javascript

fn main() {
    let mut s1 = String::from("Hello");
    update_word(&mut s1);
    println!("{}", s1);
}

fn update_word(word: &mut String) {
    word.push_str(" World");
}
```

Try having more than one mutable reference at the same time -
```javascript
fn main() {
    let mut s1 = String::from("Hello");
    let s2 = &mut s1;
    update_word(&mut s1);
    println!("{}", s1);
    println!("{}", s2);
}

fn update_word(word: &mut String) {
    word.push_str(" World");
}
```
### Rules of borrowing
[](https://projects.100xdevs.com/tracks/rust-bootcamp/Rust-Bootcamp-14#0b8da49d3cb74e5b9b177a3daffbcc39 "Rules of borrowing")Rules of borrowing
- There can me many `immutable references` at the same time
```javascript

fn main() {
    let  s1 = String::from("Hello");
    let s2 = &s1;
    let s3 = &s1;
    
    println!("{}", s1);
    println!("{}", s2);
    println!("{}", s3);
}
// No errors
```

- There can be only one `mutable reference` at a time
```javascript
fn main() {
    let mut s1 = String::from("Hello");
    let s2 = &mut s1;
    let s3 = update_word(&mut s1);
    
    println!("{}", s1);
    println!("{}", s2);
}

fn update_word(word: &mut String) {
    word.push_str(" World");
}
// Error
```

- If there is a `mutable reference` , you can’t have another immutable reference either.
```javascript
fn main() {
    let mut s1 = String::from("Hello");
    let s2 = &mut s1;
    let s3 = &s1;
    
    println!("{}", s1);
    println!("{}", s2);
}

fn update_word(word: &mut String) {
    word.push_str(" World");
}
```
- This to avoid any data races/inconsistent behaviour
- If someone makes an `immutable reference` , they don’t expect the value to change suddenly
- If more than one `mutable references` happen, there is a possibility of a data race and synchronization issues

Two good things to discuss at this point should be **but we’re going to ignore it for now 1.** `**Lifetimes**` **2. String slices (&str)**
Ref - [https://doc.rust-lang.org/book/ch04-03-slices.html](https://doc.rust-lang.org/book/ch04-03-slices.html)