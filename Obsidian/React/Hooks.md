## 🎣 Basic Hooks

These are the Hooks you'll use most often:
### 1. `useState`
- **Purpose:** Allows functional components to manage **state**. It returns a pair of values: the current state and a function that updates it.
- **Syntax:** `const [state, setState] = useState(initialValue);`
- **Code Example:**
JavaScript
```js
import React, { useState } from 'react';

function Counter() {
  // Declares a state variable named 'count', initialized to 0.
  // 'setCount' is the function to update 'count'.
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

### 2. `useEffect`
- **Purpose:** Allows you to perform **side effects** in your functional components, such as data fetching, subscriptions, or manually changing the DOM. It runs after every render by default.
- **Syntax:** `useEffect(() => { /* side effect */ }, [dependencies]);`
- **Dependencies Array (`[dependencies]`):**
    - **No array (omitted):** Effect runs after _every_ render.
    - **Empty array (`[]`):** Effect runs only **once** after the initial render (like `componentDidMount`).
    - **Array with values (`[propA, stateB]`):** Effect runs on the initial render and whenever any of the listed dependencies change.
- **Code Example (Runs once after initial render):**

JavaScript
```js
import React, { useState, useEffect } from 'react';

function DataFetcher() {
  const [data, setData] = useState(null);

  // The empty array dependency means this runs only once,
  // typically used for fetching initial data.
  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(res => res.json())
      .then(setData);
      
    // Optional: Return a cleanup function (e.g., to clear a timer)
    // return () => { /* cleanup code */ };
  }, []); 

  return <div>{data ? JSON.stringify(data) : 'Loading...'}</div>;
}
```
### 3. `useContext`
- **Purpose:** Allows you to read the context value and subscribe to context changes. Context provides a way to pass data through the component tree without having to pass props down manually at every level (prop drilling).
- **Syntax:** `const value = useContext(MyContext);`
- **Code Example:**
JavaScript
```js
// 1. Create the Context (ThemeContext.js)
import React, { createContext } from 'react';
export const ThemeContext = createContext('light');

// 2. Use the Context (App.js)
import { ThemeContext } from './ThemeContext';
import Toolbar from './Toolbar';

function App() {
  // The Provider wraps components that need access to the context.
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

// 3. Consume the Context (Toolbar.js)
import { useContext } from 'react';
import { ThemeContext } from './ThemeContext';

function Toolbar() {
  // Read the current context value ("dark")
  const theme = useContext(ThemeContext); 
  
  return (
    <button style={{ background: theme === 'dark' ? 'black' : 'white', color: theme === 'dark' ? 'white' : 'black' }}>
      Current Theme: {theme}
    </button>
  );
}
```

---
## 🛠️ Additional Hooks
These Hooks are primarily used for performance optimization or for managing more complex state logic or DOM interactions.
### 4. `useReducer`
- **Purpose:** An alternative to `useState` for more complex state logic that involves multiple sub-values or when the next state depends on the previous one. It's often preferred for state management that involves a clear "action" type, similar to Redux.
- **Syntax:** `const [state, dispatch] = useReducer(reducer, initialState);`
- **Code Example:**
JavaScript
```js
import React, { useReducer } from 'react';

const initialState = { count: 0 };

// The reducer function takes the current state and an action, and returns the new state.
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return initialState;
    default:
      throw new Error();
  }
}

function Counter() {
  // 'dispatch' is the function used to send actions to the reducer.
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </>
  );
}
```
### 5. `useMemo`
- **Purpose:** Returns a **memoized value**. It only re-calculates the value when one of its dependencies changes. This is useful for **optimizing performance** by avoiding expensive calculations on every render.
- **Syntax:** `const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);`
- **Code Example:**
JavaScript

```js
import React, { useState, useMemo } from 'react';

// Simulates an expensive, slow function
function computeExpensiveValue(num) {
  console.log('Calculating...');
  for (let i = 0; i < 1000000000; i++) {
    num += 1;
  }
  return num;
}

function Example() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');

  // 'expensiveResult' will only be re-calculated when 'count' changes.
  // It won't re-calculate when 'text' changes.
  const expensiveResult = useMemo(() => computeExpensiveValue(count), [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
      
      <p>Text: {text}</p>
      <input type="text" value={text} onChange={(e) => setText(e.target.value)} />

      <p>Expensive Result: {expensiveResult}</p>
    </div>
  );
}
```
### 6. `useCallback`
#readmore- https://react.dev/reference/react/useCallback

- **Purpose:** Returns a **memoized callback function**. This prevents a function from being recreated on every render, which is crucial when passing callbacks to optimized child components that rely on reference equality (like those wrapped in `React.memo`).
- **Syntax:** `const memoizedCallback = useCallback(() => { /* function body */ }, [dependencies]);`
- **Code Example:**
JavaScript
```js
import React, { useState, useCallback } from 'react';

// This child component will only re-render if its props change
const Button = React.memo(({ handleClick }) => {
  console.log('Button rendered');
  return <button onClick={handleClick}>Click Me</button>;
});

function ParentComponent() {
  const [count, setCount] = useState(0);

  // 'handleClick' will only be recreated if the 'count' dependency changes.
  // This prevents the 'Button' component from re-rendering unnecessarily
  // when 'count' changes, because the 'handleClick' function reference remains the same.
  const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]); // Dependency on 'count' is necessary for it to update

  return (
    <div>
      <p>Count: {count}</p>
      <Button handleClick={handleClick} />
    </div>
  );
}
```
### 7. `useRef`
- **Purpose:** Returns a mutable **ref object** whose `.current` property is initialized to the passed argument. The returned object will persist for the full lifetime of the component. It is commonly used for:
    - Accessing the DOM element directly (e.g., focusing an input).
    - Storing a mutable value that doesn't cause a re-render when updated (e.g., a timer ID).
- **Syntax:** `const refObject = useRef(initialValue);`
- **Code Example:**
JavaScript

```js
import React, { useRef } from 'react';

function TextInputWithFocusButton() {
  // 'inputEl' will hold a reference to the DOM input element
  const inputEl = useRef(null); 

  const onButtonClick = () => {
    // Accessing the DOM element and calling the native focus() method
    inputEl.current.focus(); 
  };

  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

## The Rules of React Hooks
There are two main rules you **must** follow when using React Hooks:
### 1. Only Call Hooks at the Top Level
- **Do not** call Hooks inside loops, conditions, or nested functions.
- **Why?** React relies on the order in which Hooks are called during each render to correctly associate state (from `useState`), effects (from `useEffect`), and other values with a component. If you put a Hook inside a condition, that Hook might not be called on every render, disrupting the expected order and causing bugs or unexpected behavior.

|**✅ Correct (Top Level)**|**❌ Incorrect (Inside a condition)**|
|---|---|
|`jsx<br/>function MyComponent() {<br/> const [count, setCount] = useState(0); // ✅ OK<br/><br/> if (count > 0) {<br/> // Other logic is fine<br/> }<br/> // ...<br/>}`|`jsx<br/>function MyComponent(props) {<br/> if (props.isLoggedIn) {<br/> const [username, setUsername] = useState(''); // ❌ BAD<br/> }<br/> // ...<br/>}`|

### 2. Only Call Hooks from React Functions
- **Do not** call Hooks from regular JavaScript functions.
- **You can only call Hooks from:**
    1. **React Functional Components** (`function MyComponent() { ... }`).
    2. **Custom Hooks** (`function useMyCustomHook() { ... }`).
- **Why?** Hooks are designed to "hook into" the internal state and lifecycle mechanisms of a React component. They need the context of a component being rendered to work.

|**✅ Correct (Inside a React Component)**|**❌ Incorrect (Inside a regular JS function)**|
|---|---|
|`jsx<br/>function MyComponent() {<br/> const [value, setValue] = useState(0); // ✅ OK<br/> // ...<br/>}`|`jsx<br/>function createCounter() {<br/> const [value, setValue] = useState(0); // ❌ BAD<br/> return { value, setValue };<br/>}`|


