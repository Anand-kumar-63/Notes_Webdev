Props (short for **properties**) are a mechanism for passing data from a **parent component** down to a **child component** in React. They are the main way to make components dynamic and reusable.

## 🎁 Key Characteristics of Props
- **Read-Only:** Props are **immutable** within the child component that receives them. A child component should never modify the props it receives; only the parent component should manage and update that data. This is often described as "unidirectional data flow."

- **Data Flow:** Data flows **downwards** (from parent to child). A child component cannot directly pass props back up to its parent.
    
- **Types:** Props can be any valid JavaScript value, including:
    
    - Strings (`"Hello"`)
    - Numbers (`42`)
    - Booleans (`true`/`false`)
    - Arrays (`[1, 2, 3]`)
    - Objects (`{ key: 'value' }`)
    - Functions (used for passing callbacks to handle events in the child)
    - React Elements (like other components)
        
---
## 👩‍💻 How to Use Props
### 1. Passing Props (In the Parent Component)
You pass props to a child component just like you would pass attributes to an HTML element.
JavaScript
```js
// ParentComponent.js
import ChildComponent from './ChildComponent';

function ParentComponent() {
  const userName = "Alice";
  const userAge = 30;

  return (
    <div>
      <ChildComponent
        name={userName} // Passing a string variable
        age={userAge}   // Passing a number variable
        isLoggedIn={true} // Passing a boolean
      />
    </div>
  );
}
```

### 2. Receiving and Using Props (In the Child Component)
The child component receives the props as a single JavaScript object, usually named `props`. You can access the individual values using dot notation (`props.name`, `props.age`) or, more commonly, using **destructuring**.
#### **Using Destructuring (Recommended for clarity):**

```js
// ChildComponent.js (Functional Component)
function ChildComponent({ name, age, isLoggedIn }) {
  return (
    <div>
      <h2>Hello, {name}!</h2>
      <p>Age: {age}</p>
      {isLoggedIn ? <p>Status: Logged In</p> : <p>Status: Logged Out</p>}
    </div>
  );
}
export default ChildComponent;
```
#### **Using the `props` object:**
JavaScript
```js
// ChildComponent.js (Functional Component)
function ChildComponent(props) {
  return (
    <div>
      <h2>Hello, {props.name}!</h2>
      <p>Age: {props.age}</p>
      {props.isLoggedIn ? <p>Status: Logged In</p> : <p>Status: Logged Out</p>}
    </div>
  );
}
export default ChildComponent;
```

#example--
## 1. Passing a Callback Function (Child Communicates Up) 📞

The most common "complex" use case is passing a function down as a prop, allowing the **child component to trigger an action in the parent component**. This is how you handle events (like button clicks or form submissions) in a component that owns the data.
### Example: A Counter Component
- **Parent Component** (`CounterContainer.js`): Manages the state (`count`) and the function to update it (`increment`).
- **Child Component** (`IncrementButton.js`): Renders the button and calls the function passed via props when clicked.
    
JavaScript
```js
// --- 1. Parent Component (Container) ---
import React, { useState } from 'react';
import IncrementButton from './IncrementButton';

function CounterContainer() {
  const [count, setCount] = useState(0);

  // Function defined in the Parent
  const handleIncrement = (step) => {
    // This action changes the state in the Parent
    setCount(prevCount => prevCount + step);
  };

  return (
    <div>
      <h3>Current Count: {count}</h3>
      {/* Passing the function down as a prop named 'onClickAction' */}
      <IncrementButton
        onClickAction={handleIncrement}
        step={5} // Also passing data down
      />
    </div>
  );
}
// ----------------------------------------
```
JavaScript
```js
// --- 2. Child Component (IncrementButton) ---
function IncrementButton({ onClickAction, step }) {
  const handleClick = () => {
    // The Child calls the Parent's function, passing the 'step' data up.
    onClickAction(step); 
  };

  return (
    <button onClick={handleClick}>
      Add {step} to Count
    </button>
  );
}
export default IncrementButton;
// ----------------------------------------
```

---
## 2. Using the Special `children` Prop 🧩
In React, the content placed **between the opening and closing tags** of a component is passed to that component as a prop called `children`. This is used to create **layout, wrapper, or structural components** that don't care about the content, only how to display it.
### Example: A Card Wrapper
- **Child Component** (`Card.js`): A generic container that applies consistent styling.
- **Parent Component** (`App.js`): Places arbitrary content inside the `Card` component.
    
JavaScript
```js
// --- 1. Child Component (Card) ---
// It simply renders anything passed between its tags
function Card({ title, children }) {
  return (
    <div style={{ border: '1px solid gray', padding: '15px', margin: '10px' }}>
      {title && <h3>{title}</h3>}
      <div style={{ marginTop: '10px' }}>
        {/* The component renders whatever is placed inside its tags */}
        {children} 
      </div>
    </div>
  );
}
export default Card;
// ----------------------------------------
```
JavaScript
```js
// --- 2. Parent Component (App) ---
import Card from './Card';

function App() {
  return (
    <div>
      <Card title="User Profile">
        {/* ALL of this JSX is passed as the 'children' prop */}
        <p>Name: **Sarah Connor**</p>
        <button>View Details</button>
      </Card>

      <Card>
        {/* You can pass different content, making Card reusable */}
        <img src="avatar.jpg" alt="Avatar" style={{ width: '100px' }} />
        <p style={{ fontStyle: 'italic' }}>Productivity tools for developers.</p>
      </Card>
    </div>
  );
}
// ----------------------------------------
```

---
## 3. Passing Data Structures and Complex JSX 📊
Props can carry rich data, like arrays of objects, which the child component can then map over (iterate) to render lists of elements.
### Example: A List Component
- **Parent Component** (`App.js`): Defines a complex data array.
- **Child Component** (`UserList.js`): Receives the array and uses the `map` method to render multiple `<li>` elements.
    
JavaScript
```js
// --- 1. Parent Component (App) ---
import UserList from './UserList';

function App() {
  const userData = [
    { id: 1, name: 'Jessie', role: 'Developer' },
    { id: 2, name: 'Walter', role: 'Manager' },
    { id: 3, name: 'Saul', role: 'Consultant' },
  ];

  return (
    <div>
      {/* Passing the full array of objects */}
      <UserList users={userData} listTitle="Company Directory" />
    </div>
  );
}
// ----------------------------------------
```

JavaScript
```js
// --- 2. Child Component (UserList) ---
function UserList({ users, listTitle }) {
  return (
    <section>
      <h2>{listTitle}</h2>
      <ul>
        {/* Mapping over the array prop to create list items */}
        {users.map(user => (
          // Key prop is essential when mapping over lists!
          <li key={user.id}>
            **{user.name}** - *{user.role}*
          </li>
        ))}
      </ul>
    </section>
  );
}
export default UserList;
// ----------------------------------------
```


# Props Drilling 
## 1. Using React Context (The Global Solution) 🌍

**React Context** is designed to share data that can be considered "global" for a tree of React components, such as the current authenticated user, theme (light/dark), or language. It allows data to be injected directly into any component within a provider's scope, bypassing the need to pass props down through intermediate levels.

### How it Solves Prop Drilling

Instead of drilling data through `ComponentA` $\rightarrow$ `ComponentB` $\rightarrow$ `ComponentC`, Context allows `ComponentC` to access the data directly from the Context Provider placed higher up, say at the level of `ComponentA`.

$$\text{Traditional Props: } \text{A} \xrightarrow{\text{data}} \text{B} \xrightarrow{\text{data}} \text{C}$$

$$\text{React Context: } \text{Provider}_{\text{Data}} \begin{cases} \text{A} \\ \text{B} \\ \text{C} \xrightarrow{\text{useContext}} \text{Data} \end{cases}$$

### Steps to Implement Context
1. **Create the Context:**
    JavaScript
    ```js
    // ThemeContext.js
    import { createContext } from 'react';
    export const ThemeContext = createContext(null);
    ```
2. **Provide the Context (Higher Level):** Wrap the parent component where the data originates with the `Provider`.
    JavaScript
    ```js
    // App.js
    import { ThemeContext } from './ThemeContext';
    
    function App() {
      const themeData = { mode: 'dark', color: 'blue' };
      return (
        // The value prop is the data accessible to all descendants
        <ThemeContext.Provider value={themeData}>
          <ComponentA /> {/* Data is now available here and below */}
        </ThemeContext.Provider>
      );
    }
    ```
1. **Consume the Context (Deeply Nested Child):** Use the `useContext` hook to access the data directly, skipping all intermediate props.
    JavaScript
    ```js
        // ComponentC.js (Deeply Nested)
    import { useContext } from 'react';
    import { ThemeContext } from './ThemeContext';
    
    function ComponentC() {
      const theme = useContext(ThemeContext);
    
      // Access data directly, no props needed!
      return (
        <div style={{ backgroundColor: theme.color }}>
          The current theme mode is **{theme.mode}**.
        </div>
      );
    }
    ```
---
## 2. Component Composition (The Structuring Solution) 🏗️
**Composition** is a more idiomatic React solution that avoids prop drilling by changing **who** is responsible for rendering the deeply nested component. Instead of the intermediate component rendering the final content, the **parent** renders and passes that content down using the `children` prop (or other named props that accept JSX).

### How it Solves Prop Drilling
You pass the final, fully configured JSX element directly past the intermediate components. The intermediate components only act as **layout wrappers**.
### Example: Avoiding Drilling through `ComponentB`
Let's say `ComponentA` has the data, and `ComponentC` needs it. `ComponentB` is an intermediate layout container.

**Problem (Prop Drilling):**
JavaScript
```js
// ComponentA
<ComponentB user={user}>

// ComponentB
<div className="layout">
  <ComponentC user={props.user} /> {/* B passes data it doesn't need */}
</div>
```

**Solution (Composition):**
JavaScript
```js
// --- 1. Intermediate ComponentB (Accepts 'children') ---
function ComponentB({ children }) {
  // B only acts as a wrapper, it doesn't know or care about 'user' data
  return (
    <div className="layout-wrapper">
      {children} 
    </div>
  );
}
// ----------------------------------------
```
JavaScript
```js
// --- 2. Parent ComponentA (Passes C as 'children') ---
function ComponentA({ user }) {
  return (
    <ComponentB>
      {/* ComponentA renders ComponentC directly and passes the 'user' prop */}
      <ComponentC user={user} />
    </ComponentB>
  );
}
// ----------------------------------------
``` 