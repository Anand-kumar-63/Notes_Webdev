Java memory model 
## Java Memory Model (High-Level Overview)
Java memory is broadly divided into these main areas:
1. **Heap**
    - Where **objects** are created (`new String("...")`, arrays, etc.).
    - Managed by the **Garbage Collector**.
2. **Method Area / Metaspace (formerly part of PermGen)**
    - Stores class metadata and **String Pool** (for literals).
    - Shared across all threads.
3. **Stack (Thread Stack)**
    - Every thread has **its own stack**.
    - Stores **method calls**, **local variables**, and **references** (pointers) to heap/pool objects.
4. **PC Register & Native Method Stack** (not directly relevant here)

## How are variables like `x`, `y`, `z` stored in the stack?
```java
public class Main {
    public static void main(String[] args) {
        String x = "Anand";
        String y = "Anand";
        String z = "Anand";
        String first = new String("Anand");
    }
}
```
- `main()` method starts execution → a new **stack frame** for `main()` is created on the **main thread's call stack**.
- Inside this frame, the variables `x`, `y`, `z`, and `first` are created as **references**.
- They are stored on the **main thread's stack**, pointing to either:
    - A **string literal** in the **String Pool** (`x`, `y`, `z`)
    - A **heap object** (`first`)
    - 
* There is no user-defined name for stack it is simply called **The Java Stack** or **Thread Stack**