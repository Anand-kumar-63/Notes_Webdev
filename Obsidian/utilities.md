Yes, the **$\text{new URL()}$ constructor returns a $\text{URL}$ object**.
This $\text{URL}$ object is an instance of the built-in JavaScript $\text{URL}$ interface, and it contains various **properties** that represent the different parts of the URL.
### 🔑 Key Characteristics of the Returned Object
- **Type:** It is a $\text{URL}$ object.
- **Purpose:** It allows you to **read, modify, and manage** the components of the URL in an organized way.
- **Properties:** It has properties like $\text{protocol}$, $\text{hostname}$, $\text{pathname}$, $\text{search}$, $\text{hash}$, and the very useful $\text{searchParams}$ (which is another object of type $\text{URLSearchParams}$).

---
### 📝 Example
If you run the constructor:
JavaScript
```
const myUrl = new URL('https://example.com/products/101?color=blue#specs');
```
The $\text{myUrl}$ variable holds the $\text{URL}$ object, allowing you to access its parts:
JavaScript
```
console.log(typeof myUrl);        // 'object'
console.log(myUrl.protocol);      // 'https:'
console.log(myUrl.hostname);      // 'example.com'
console.log(myUrl.pathname);      // '/products/101'
console.log(myUrl.searchParams);  // URLSearchParams { 'color' => 'blue' }
console.log(myUrl.href);          // 'https://example.com/products/101?color=blue#specs'
```
The object structure ensures that the URL is always valid and correctly encoded when you access the $\text{href}$ or $\text{toString()}$ properties after making any changes.