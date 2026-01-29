## 🚀 Complete React Learning Path: Zero to Production-Ready

### Lesson 1.1: What is React & Why It Exists

**React** is a JavaScript library for building user interfaces using **components** (reusable UI pieces). Think of it like LEGO blocks.

**Key concepts:**

- **Declarative**: Describe _what_ UI should look like
- **Component-based**: Break UI into reusable pieces
- **Virtual DOM**: Efficient updates by comparing changes

**Why React dominates 2025:**

- Huge ecosystem (Next.js, Vite, React Native)
- Server Components for performance
- Industry standard

### Lesson 1.2: Modern Setup (Vite)

**Skip Create React App** - use **Vite**:

---

## **Module 1: Foundation \& Mental Model**

### **Lesson 1.1: What is React \& Why It Exists**

**Simple explanation:**
React is a JavaScript library for building user interfaces using **components** (reusable pieces of UI). Think of it like LEGO blocks—you create small pieces and combine them to build complex applications.

**Key concepts:**

- **Declarative**: You describe _what_ the UI should look like, React handles _how_ to update it
- **Component-based**: Break UI into independent, reusable pieces
- **Virtual DOM**: React creates a lightweight copy of the actual DOM, compares changes, and updates only what's necessary (efficient!)

**Why React dominates in 2025:**

- Huge ecosystem (Next.js, Vite, React Native)
- Server Components for performance
- Excellent developer experience
- Industry standard (most job postings)

---

### **Lesson 1.2: Modern Setup (Vite - NOT Create React App)**

**Important:** Create React App is outdated in 2025. Use **Vite** for learning and client-side apps.[^1_1]

**Setup steps:**

```bash
# Install Node.js first (18+ recommended)
# Then create a React project with Vite:

npm create vite@latest my-react-app -- --template react
cd my-react-app
npm install
npm run dev
```

**Project structure you'll see:**

```
my-react-app/
├── src/
│   ├── App.jsx       # Main component
│   ├── main.jsx      # Entry point
│   └── App.css       # Styles
├── index.html        # HTML template
├── package.json      # Dependencies
└── vite.config.js    # Vite configuration
```

**Why Vite:**

- Lightning-fast hot module replacement (HMR)[^1_1]
- Instant server startup
- Modern ES modules
- Production-ready build tool

---

### **Lesson 1.3: Your First Component**

Open `src/App.jsx` and replace everything:

```jsx
function App() {
  return (
    <div>
      <h1>Hello React!</h1>
      <p>My first component</p>
    </div>
  );
}

export default App;
```

**Key principles:**

1. **Components are functions** that return JSX (looks like HTML, but it's JavaScript)
2. **JSX must return ONE parent element** (that's why we wrap in `<div>`)
3. **Export** your component so others can use it

**Exercise 1.3:**
Create a `Greeting` component that displays your name:

```jsx
function Greeting() {
  return <h2>Welcome, [Your Name]!</h2>;
}

function App() {
  return (
    <div>
      <Greeting />
      <Greeting />
    </div>
  );
}
```

Notice: You can reuse components! That's the power of React.

---

## **Module 2: JSX \& Props (Component Communication)**

### **Lesson 2.1: JSX Rules**

JSX looks like HTML but has different rules:

```jsx
function Example() {
  const name = "John";
  const isLoggedIn = true;

  return (
    <div>
      {/* 1. Use {} for JavaScript expressions */}
      <h1>Hello {name}</h1>

      {/* 2. className instead of class */}
      <p className="text-large">Text</p>

      {/* 3. Conditional rendering */}
      {isLoggedIn ? <p>Welcome back!</p> : <p>Please login</p>}

      {/* 4. Self-closing tags need / */}
      <img src="photo.jpg" alt="Description" />

      {/* 5. camelCase for attributes */}
      <button onClick={() => alert("Clicked!")}>Click</button>
    </div>
  );
}
```

**Common mistakes:**

```
- ❌ `<div class="box">` → ✅ `<div className="box">`
```

```
- ❌ `<img src="">` → ✅ `<img src="" />`
```

- ❌ `onclick` → ✅ `onClick`

---

### **Lesson 2.2: Props (Passing Data to Components)**

**Props** are like function parameters—they let you customize components.

```jsx
// Component that accepts props
function UserCard(props) {
  return (
    <div className="card">
      <h3>{props.name}</h3>
      <p>Age: {props.age}</p>
      <p>Role: {props.role}</p>
    </div>
  );
}

// Using the component
function App() {
  return (
    <div>
      <UserCard name="Alice" age={25} role="Developer" />
      <UserCard name="Bob" age={30} role="Designer" />
    </div>
  );
}
```

**Modern way (destructuring):**

```jsx
function UserCard({ name, age, role }) {
  return (
    <div className="card">
      <h3>{name}</h3>
      <p>Age: {age}</p>
      <p>Role: {role}</p>
    </div>
  );
}
```

**Props rules:**

- **Read-only** (never modify props inside a component)
- Can pass any data type (strings, numbers, arrays, objects, functions)
- Use numbers/booleans in `{}`: `<Card age={25} active={true} />`

**Exercise 2.2:**
Create a `ProductCard` component with props: `title`, `price`, `inStock` (boolean). Display "Available" or "Sold Out" based on `inStock`.

---

## **Module 3: State \& Interactivity (useState)**

### **Lesson 3.1: Introduction to State**

**State** is data that changes over time. When state changes, React re-renders the component.

**First hook: useState**[^1_2]

```jsx
import { useState } from "react";

function Counter() {
  // [current value, function to update it] = useState(initial value)
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

**How it works:**

1. `useState(0)` creates state variable with initial value `0`
2. `count` is the current value
3. `setCount` is the function to update it
4. When you call `setCount`, React re-renders the component with new value

---

### **Lesson 3.2: Multiple State Variables**

**Best practice:** Use separate state variables for unrelated data[^1_2]

```jsx
function Form() {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const [age, setAge] = useState(0);

  return (
    <div>
      <input
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Name"
      />
      <input
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
      />
      <input
        type="number"
        value={age}
        onChange={(e) => setAge(Number(e.target.value))}
        placeholder="Age"
      />
      <p>
        Hello {name}, you are {age} years old!
      </p>
    </div>
  );
}
```

**When to use single vs multiple state:**

- Related data that updates together → Single object
- Independent values → Separate state variables

---

### **Lesson 3.3: State with Objects/Arrays**

**Objects:**

```jsx
function UserProfile() {
  const [user, setUser] = useState({
    name: "John",
    age: 25,
    city: "NYC",
  });

  const updateCity = () => {
    // Create NEW object (don't mutate!)
    setUser({ ...user, city: "LA" });
  };

  return (
    <div>
      <p>
        {user.name} from {user.city}
      </p>
      <button onClick={updateCity}>Move to LA</button>
    </div>
  );
}
```

**Arrays:**

```jsx
function TodoList() {
  const [todos, setTodos] = useState(["Learn React", "Build app"]);
  const [input, setInput] = useState("");

  const addTodo = () => {
    setTodos([...todos, input]); // Create new array
    setInput("");
  };

  const removeTodo = (index) => {
    setTodos(todos.filter((_, i) => i !== index));
  };

  return (
    <div>
      <input value={input} onChange={(e) => setInput(e.target.value)} />
      <button onClick={addTodo}>Add</button>

      <ul>
        {todos.map((todo, index) => (
          <li key={index}>
            {todo}
            <button onClick={() => removeTodo(index)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

**Critical rule:** Always create NEW objects/arrays when updating state (never mutate directly).

**Exercise 3.3:**
Build a shopping cart: Add items, remove items, show total count.

---

## **Module 4: Side Effects (useEffect)**

### **Lesson 4.1: What are Side Effects?**

**Side effects** = Anything that affects the outside world:[^1_2]

- API calls (fetching data)
- Setting up timers
- Updating document title
- Subscribing to events

**useEffect structure:**

```jsx
import { useState, useEffect } from "react";

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    // This runs AFTER every render
    document.title = `Count: ${count}`;
  });

  return <button onClick={() => setCount(count + 1)}>Increment</button>;
}
```

---

### **Lesson 4.2: Dependency Array (CRITICAL CONCEPT)**

Control **when** useEffect runs with the dependency array:[^1_2]

```jsx
function Examples() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState("");

  // 1. No dependency array - runs after EVERY render
  useEffect(() => {
    console.log("Runs every render");
  });

  // 2. Empty array [] - runs ONCE after initial render
  useEffect(() => {
    console.log("Runs once (like componentDidMount)");
  }, []);

  // 3. With dependencies - runs when dependencies change
  useEffect(() => {
    console.log("Runs when count changes");
  }, [count]);

  // 4. Multiple dependencies
  useEffect(() => {
    console.log("Runs when count OR name changes");
  }, [count, name]);
}
```

**Golden rule:** List ALL variables from component scope that your effect uses.

---

### **Lesson 4.3: Data Fetching Pattern**

**Production-ready data fetching:**[^1_2]

```jsx
function UserData() {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchUser = async () => {
      try {
        setLoading(true);
        const response = await fetch("https://api.example.com/user");
        if (!response.ok) throw new Error("Failed to fetch");
        const data = await response.json();
        setUser(data);
        setError(null);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchUser();
  }, []); // Empty array = fetch once on mount

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;
  if (!user) return <p>No user data</p>;

  return (
    <div>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </div>
  );
}
```

**3-state pattern:**

1. `loading` - show spinner
2. `error` - show error message
3. `data` - show actual content

---

### **Lesson 4.4: Cleanup Functions**

**Clean up subscriptions/timers to prevent memory leaks:**

```jsx
function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds((s) => s + 1);
    }, 1000);

    // Cleanup function (runs when component unmounts)
    return () => clearInterval(interval);
  }, []);

  return <p>Seconds: {seconds}</p>;
}
```

**When cleanup runs:**

- Before component unmounts
- Before effect runs again (if dependencies changed)

**Exercise 4.4:**
Build a component that fetches posts from `https://jsonplaceholder.typicode.com/posts` and displays them. Handle loading and errors.

---

## **Module 5: Advanced Hooks**

### **Lesson 5.1: useContext (Sharing Data Without Props)**

**Problem:** Passing props through many levels ("prop drilling") is tedious.

**Solution:** Context API[^1_2]

```jsx
import { createContext, useContext, useState } from "react";

// 1. Create context
const ThemeContext = createContext();

// 2. Provider component
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");

  const toggleTheme = () => {
    setTheme(theme === "light" ? "dark" : "light");
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// 3. Consumer component
function ThemedButton() {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <button
      onClick={toggleTheme}
      style={{ background: theme === "light" ? "#fff" : "#333" }}
    >
      Toggle Theme (Current: {theme})
    </button>
  );
}

// 4. App structure
function App() {
  return (
    <ThemeProvider>
      <ThemedButton />
      {/* Any component can access theme now! */}
    </ThemeProvider>
  );
}
```

**When to use:**

- Global app state (theme, auth, language)
- Avoid passing props through 3+ levels

**2025 best practice:** Don't overuse context for everything—it causes re-renders. Use for truly global data.[^1_3]

---

### **Lesson 5.2: useReducer (Complex State Logic)**

**When state logic gets complex, useReducer is cleaner than useState:**[^1_2]

```jsx
import { useReducer } from "react";

// 1. Define reducer function
function todoReducer(state, action) {
  switch (action.type) {
    case "ADD_TODO":
      return [...state, { id: Date.now(), text: action.payload, done: false }];
    case "TOGGLE_TODO":
      return state.map((todo) =>
        todo.id === action.payload ? { ...todo, done: !todo.done } : todo
      );
    case "DELETE_TODO":
      return state.filter((todo) => todo.id !== action.payload);
    default:
      return state;
  }
}

// 2. Use in component
function TodoApp() {
  const [todos, dispatch] = useReducer(todoReducer, []);
  const [text, setText] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!text.trim()) return;
    dispatch({ type: "ADD_TODO", payload: text });
    setText("");
  };

  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input value={text} onChange={(e) => setText(e.target.value)} />
        <button type="submit">Add</button>
      </form>

      <ul>
        {todos.map((todo) => (
          <li key={todo.id}>
            <input
              type="checkbox"
              checked={todo.done}
              onChange={() =>
                dispatch({ type: "TOGGLE_TODO", payload: todo.id })
              }
            />
            <span
              style={{ textDecoration: todo.done ? "line-through" : "none" }}
            >
              {todo.text}
            </span>
            <button
              onClick={() =>
                dispatch({ type: "DELETE_TODO", payload: todo.id })
              }
            >
              Delete
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

**useReducer vs useState:**

- Simple state (single value) → `useState`
- Complex state (multiple related fields, complex updates) → `useReducer`

---

### **Lesson 5.3: useMemo (Performance - Expensive Calculations)**

**useMemo caches expensive calculations:**[^1_3][^1_2]

```jsx
import { useState, useMemo } from "react";

function ExpensiveComponent({ numbers }) {
  const [filter, setFilter] = useState("");

  // Without useMemo: recalculates on EVERY render
  // With useMemo: only recalculates when dependencies change
  const filteredAndSorted = useMemo(() => {
    console.log("Computing...");
    return numbers
      .filter((n) => n.toString().includes(filter))
      .sort((a, b) => a - b);
  }, [numbers, filter]);

  return (
    <div>
      <input
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
        placeholder="Filter numbers"
      />
      <ul>
        {filteredAndSorted.map((n) => (
          <li key={n}>{n}</li>
        ))}
      </ul>
    </div>
  );
}
```

**When to use:**

- Expensive calculations (loops over large arrays)
- Derived data from props/state
- **Don't overuse**—premature optimization! Profile first[^1_3]

---

### **Lesson 5.4: useCallback (Memoizing Functions)**

**useCallback prevents function recreation:**[^1_3][^1_2]

```jsx
import { useState, useCallback } from "react";

function Parent() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");

  // Without useCallback: new function on every render
  // With useCallback: same function reference
  const handleClick = useCallback(() => {
    console.log("Clicked with count:", count);
  }, [count]);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <ChildComponent onClick={handleClick} />
    </div>
  );
}

// This won't re-render unless onClick reference changes
const ChildComponent = React.memo(({ onClick }) => {
  console.log("Child rendered");
  return <button onClick={onClick}>Child Button</button>;
});
```

**Use with React.memo:**

- `useCallback` keeps function reference stable
- `React.memo` prevents child re-renders when props haven't changed

---

### **Lesson 5.5: Custom Hooks (Reusable Logic)**

**Extract reusable logic into custom hooks:**[^1_4]

```jsx
// Custom hook for form inputs
function useFormInput(initialValue) {
  const [value, setValue] = useState(initialValue);

  const handleChange = (e) => {
    setValue(e.target.value);
  };

  const reset = () => setValue(initialValue);

  return {
    value,
    onChange: handleChange,
    reset,
  };
}

// Usage in component
function LoginForm() {
  const email = useFormInput("");
  const password = useFormInput("");

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("Login:", email.value, password.value);
    email.reset();
    password.reset();
  };

  return (
    <form onSubmit={handleSubmit}>
      <input {...email} type="email" placeholder="Email" />
      <input {...password} type="password" placeholder="Password" />
      <button type="submit">Login</button>
    </form>
  );
}
```

**Another example - API fetching:**

```jsx
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        if (!response.ok) throw new Error("Failed to fetch");
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
}

// Usage
function Users() {
  const { data, loading, error } = useFetch("https://api.example.com/users");

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {data.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

**Custom hook rules:**

- Always start with `use` (e.g., `useFormInput`, `useFetch`)
- Can use other hooks inside
- Share stateful logic, not state itself

**Exercise 5.5:**
Create `useLocalStorage` hook that syncs state with localStorage.

---

## **Module 6: Performance Optimization**

### **Lesson 6.1: Understanding Re-renders**

**What triggers re-renders:**[^1_5][^1_3]

1. State changes (`useState`, `useReducer`)
2. Props change
3. Parent component re-renders
4. Context value changes

**Example problem:**

```jsx
function Parent() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      <ExpensiveChild /> {/* Re-renders even though it doesn't use count! */}
    </div>
  );
}
```

---

### **Lesson 6.2: React.memo (Prevent Unnecessary Re-renders)**

**React.memo skips re-renders when props haven't changed:**[^1_6][^1_3]

```jsx
import { memo } from "react";

const ExpensiveChild = memo(function ExpensiveChild({ data }) {
  console.log("ExpensiveChild rendered");

  // Expensive calculation
  const result = data.map(/* ... */);

  return <div>{/* render result */}</div>;
});

// Now only re-renders when 'data' prop changes
```

**When to use:**

- Component renders often with same props
- Component is expensive to render
- Pure components (same props = same output)

**Don't overuse:** Adds overhead. Profile first.[^1_3]

---

### **Lesson 6.3: Code Splitting \& Lazy Loading**

**Load components only when needed:**[^1_5]

```jsx
import { lazy, Suspense } from "react";

// Instead of: import Dashboard from './Dashboard';
const Dashboard = lazy(() => import("./Dashboard"));
const Settings = lazy(() => import("./Settings"));

function App() {
  const [page, setPage] = useState("home");

  return (
    <div>
      <nav>
        <button onClick={() => setPage("home")}>Home</button>
        <button onClick={() => setPage("dashboard")}>Dashboard</button>
        <button onClick={() => setPage("settings")}>Settings</button>
      </nav>

      <Suspense fallback={<p>Loading...</p>}>
        {page === "dashboard" && <Dashboard />}
        {page === "settings" && <Settings />}
      </Suspense>
    </div>
  );
}
```

**Benefits:**

- Smaller initial bundle
- Faster page load
- Load features on-demand

---

### **Lesson 6.4: Virtualization (Large Lists)**

**For lists with 1000+ items, render only visible ones:**[^1_3]

```bash
npm install react-window
```

```jsx
import { FixedSizeList } from "react-window";

function LargeList({ items }) {
  const Row = ({ index, style }) => (
    <div style={style}>
      Item {index}: {items[index]}
    </div>
  );

  return (
    <FixedSizeList
      height={600}
      itemCount={items.length}
      itemSize={50}
      width="100%"
    >
      {Row}
    </FixedSizeList>
  );
}
```

---

### **Lesson 6.5: Performance Profiling**

**Use React DevTools Profiler:**[^1_5][^1_3]

1. Install React DevTools browser extension
2. Open DevTools → Profiler tab
3. Click record, interact with app, stop recording
4. See which components rendered and how long they took

**Optimization checklist 2025:**[^1_3]

- ✅ Use production builds for deployment
- ✅ Code splitting with `React.lazy`
- ✅ Memoize expensive calculations (`useMemo`)
- ✅ Memoize callback functions (`useCallback`)
- ✅ Wrap pure components with `React.memo`
- ✅ Virtualize long lists
- ✅ Optimize images (WebP, lazy loading)
- ✅ Use keys properly in lists

**Exercise 6.5:**
Build a list with 10,000 items. Compare performance with and without virtualization.

---

## **Module 7: Project Structure \& Best Practices**

### **Lesson 7.1: Modern React Project Structure (2025)**

**Feature-based structure (recommended for scalable apps):**[^1_7]

```
src/
├── features/
│   ├── auth/
│   │   ├── components/
│   │   │   ├── LoginForm.jsx
│   │   │   └── SignupForm.jsx
│   │   ├── hooks/
│   │   │   └── useAuth.js
│   │   └── api/
│   │       └── authApi.js
│   ├── posts/
│   │   ├── components/
│   │   │   ├── PostList.jsx
│   │   │   └── PostItem.jsx
│   │   ├── hooks/
│   │   │   └── usePosts.js
│   │   └── api/
│   │       └── postsApi.js
│   └── profile/
│       ├── components/
│       └── hooks/
├── components/        # Shared components
│   ├── Button.jsx
│   ├── Card.jsx
│   └── Modal.jsx
├── hooks/            # Shared hooks
│   ├── useFetch.js
│   └── useLocalStorage.js
├── context/          # Global context
│   └── ThemeContext.jsx
├── utils/            # Helper functions
│   ├── formatDate.js
│   └── validation.js
├── App.jsx
└── main.jsx
```

**Key principles:**[^1_7]

- Group by feature, not by file type
- Keep related code together
- Shared code goes in top-level folders
- Max 2-3 levels of nesting

---

### **Lesson 7.2: Component Best Practices**

**1. Keep components small and focused:**

```jsx
// ❌ Bad - doing too much
function UserDashboard() {
  // 200 lines of code handling user, posts, comments, settings...
}

// ✅ Good - single responsibility
function UserDashboard() {
  return (
    <div>
      <UserHeader />
      <UserStats />
      <UserPosts />
      <UserSettings />
    </div>
  );
}
```

**2. Extract reusable logic into custom hooks:**

```jsx
// ✅ Clean component using custom hook
function TodoList() {
  const { todos, addTodo, removeTodo, toggleTodo } = useTodos();

  return (/* JSX using todos */);
}
```

**3. Props validation (TypeScript or PropTypes):**

```jsx
// Using PropTypes (for JavaScript)
import PropTypes from 'prop-types';

function UserCard({ name, age, email }) {
  return (/* JSX */);
}

UserCard.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number.isRequired,
  email: PropTypes.string
};
```

**4. Avoid inline functions in JSX:**[^1_3]

```jsx
// ❌ Bad - creates new function on every render
<button onClick={() => handleClick(id)}>Click</button>;

// ✅ Good - use useCallback or extract
const handleButtonClick = useCallback(() => handleClick(id), [id]);
<button onClick={handleButtonClick}>Click</button>;
```

---

### **Lesson 7.3: Error Boundaries**

**Catch JavaScript errors in component tree:**[^1_8]

```jsx
import { Component } from "react";

class ErrorBoundary extends Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    console.error("Error caught:", error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h2>Something went wrong</h2>
          <p>{this.state.error?.message}</p>
        </div>
      );
    }

    return this.props.children;
  }
}

// Usage
function App() {
  return (
    <ErrorBoundary>
      <MyComponent />
    </ErrorBoundary>
  );
}
```

**Note:** Error boundaries are currently class-based (no hook equivalent yet).

---

## **Module 8: Routing (React Router)**

### **Lesson 8.1: Setting Up React Router**

```bash
npm install react-router-dom
```

**Basic setup:**

```jsx
import { BrowserRouter, Routes, Route, Link } from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/users">Users</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/users" element={<Users />} />
        <Route path="/users/:id" element={<UserDetail />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}
```

---

### **Lesson 8.2: Navigation \& URL Parameters**

```jsx
import { useParams, useNavigate } from "react-router-dom";

function UserDetail() {
  const { id } = useParams(); // Get URL parameter
  const navigate = useNavigate(); // Programmatic navigation

  const { data, loading } = useFetch(`/api/users/${id}`);

  if (loading) return <p>Loading...</p>;

  return (
    <div>
      <h1>{data.name}</h1>
      <button onClick={() => navigate("/users")}>Back to Users</button>
      <button onClick={() => navigate(`/users/${id}/edit`)}>Edit</button>
    </div>
  );
}
```

**Query parameters:**

```jsx
import { useSearchParams } from "react-router-dom";

function SearchPage() {
  const [searchParams, setSearchParams] = useSearchParams();

  const query = searchParams.get("q");
  const page = searchParams.get("page") || 1;

  return (
    <div>
      <input
        value={query || ""}
        onChange={(e) => setSearchParams({ q: e.target.value, page: 1 })}
      />
      <p>Searching for: {query}</p>
      <p>Page: {page}</p>
    </div>
  );
}
```

---

### **Lesson 8.3: Protected Routes**

```jsx
import { Navigate } from "react-router-dom";

function ProtectedRoute({ children }) {
  const { isAuthenticated } = useAuth(); // Custom hook

  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }

  return children;
}

// Usage
<Route
  path="/dashboard"
  element={
    <ProtectedRoute>
      <Dashboard />
    </ProtectedRoute>
  }
/>;
```

---

## **Module 9: State Management (Redux Toolkit)**

### **Lesson 9.1: When You Need Redux**

**Use Redux when:**

- Many components need same state
- State updates are complex
- You need middleware (logging, API calls)
- Team uses Redux

**Don't need Redux if:**

- Small app (Context API is enough)
- State is mostly local to components

---

### **Lesson 9.2: Redux Toolkit Setup (Modern Redux)**

```bash
npm install @reduxjs/toolkit react-redux
```

**Store setup:**

```jsx
// store/store.js
import { configureStore } from "@reduxjs/toolkit";
import todosReducer from "./todosSlice";

export const store = configureStore({
  reducer: {
    todos: todosReducer,
  },
});

// main.jsx
import { Provider } from "react-redux";
import { store } from "./store/store";

ReactDOM.createRoot(document.getElementById("root")).render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

---

### **Lesson 9.3: Creating Slices**

```jsx
// store/todosSlice.js
import { createSlice } from "@reduxjs/toolkit";

const todosSlice = createSlice({
  name: "todos",
  initialState: [],
  reducers: {
    addTodo: (state, action) => {
      state.push({ id: Date.now(), text: action.payload, done: false });
    },
    toggleTodo: (state, action) => {
      const todo = state.find((t) => t.id === action.payload);
      if (todo) todo.done = !todo.done;
    },
    deleteTodo: (state, action) => {
      return state.filter((t) => t.id !== action.payload);
    },
  },
});

export const { addTodo, toggleTodo, deleteTodo } = todosSlice.actions;
export default todosSlice.reducer;
```

---

### **Lesson 9.4: Using Redux in Components**

```jsx
import { useSelector, useDispatch } from "react-redux";
import { addTodo, toggleTodo, deleteTodo } from "./store/todosSlice";

function TodoApp() {
  const todos = useSelector((state) => state.todos);
  const dispatch = useDispatch();
  const [text, setText] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    dispatch(addTodo(text));
    setText("");
  };

  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input value={text} onChange={(e) => setText(e.target.value)} />
        <button type="submit">Add</button>
      </form>

      <ul>
        {todos.map((todo) => (
          <li key={todo.id}>
            <input
              type="checkbox"
              checked={todo.done}
              onChange={() => dispatch(toggleTodo(todo.id))}
            />
            {todo.text}
            <button onClick={() => dispatch(deleteTodo(todo.id))}>
              Delete
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

### **Lesson 9.5: Async Actions (Thunks)**

```jsx
import { createSlice, createAsyncThunk } from "@reduxjs/toolkit";

// Async thunk
export const fetchPosts = createAsyncThunk("posts/fetchPosts", async () => {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts");
  return response.json();
});

const postsSlice = createSlice({
  name: "posts",
  initialState: {
    items: [],
    status: "idle", // 'idle' | 'loading' | 'succeeded' | 'failed'
    error: null,
  },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchPosts.pending, (state) => {
        state.status = "loading";
      })
      .addCase(fetchPosts.fulfilled, (state, action) => {
        state.status = "succeeded";
        state.items = action.payload;
      })
      .addCase(fetchPosts.rejected, (state, action) => {
        state.status = "failed";
        state.error = action.error.message;
      });
  },
});

// In component
function Posts() {
  const dispatch = useDispatch();
  const { items, status, error } = useSelector((state) => state.posts);

  useEffect(() => {
    if (status === "idle") {
      dispatch(fetchPosts());
    }
  }, [status, dispatch]);

  if (status === "loading") return <p>Loading...</p>;
  if (status === "failed") return <p>Error: {error}</p>;

  return (
    <ul>
      {items.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

---

## **Module 10: Next.js (React Framework)**

### **Lesson 10.1: Why Next.js?**

**Next.js adds to React:**[^1_1]

- **Server-Side Rendering (SSR)** - Fast initial page load
- **Static Site Generation (SSG)** - Pre-render at build time
- **File-based routing** - No React Router needed
- **API routes** - Backend endpoints in same project
- **Server Components** - Run React on server (2025 feature)

**When to use:**

- SEO is critical (blogs, marketing sites)
- Need server-side logic
- Want optimized performance out-of-the-box

---

### **Lesson 10.2: Next.js Setup**

```bash
npx create-next-app@latest my-next-app
cd my-next-app
npm run dev
```

**App Router structure (Next.js 14+):**[^1_9]

```
app/
├── layout.tsx          # Root layout (wraps all pages)
├── page.tsx            # Home page (/)
├── about/
│   └── page.tsx        # About page (/about)
├── blog/
│   ├── page.tsx        # Blog list (/blog)
│   └── [slug]/
│       └── page.tsx    # Blog post (/blog/my-post)
└── api/
    └── posts/
        └── route.ts    # API endpoint (/api/posts)
```

**File = Route** (no Router configuration needed!)

---

### **Lesson 10.3: Server vs Client Components**

**2025 game-changer: Server Components run on server, send HTML to client**[^1_5]

```jsx
// app/posts/page.tsx (Server Component by default)
async function PostsPage() {
  // Fetch on server - no loading state needed!
  const posts = await fetch("https://api.example.com/posts").then((r) =>
    r.json()
  );

  return (
    <div>
      <h1>Posts</h1>
      {posts.map((post) => (
        <article key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.body}</p>
        </article>
      ))}
    </div>
  );
}
```

**Client Component (for interactivity):**

```jsx
"use client"; // Mark as client component

import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}
```

**Rules:**

- Server Components: Default, can't use hooks/event handlers
- Client Components: Add `'use client'` at top, can use hooks

---

### **Lesson 10.4: Data Fetching Patterns**

**Static Generation (build time):**

```jsx
// app/posts/[id]/page.tsx
async function PostPage({ params }) {
  const post = await fetch(`https://api.example.com/posts/${params.id}`).then(
    (r) => r.json()
  );

  return (
    <article>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
    </article>
  );
}

// Pre-generate these pages at build time
export async function generateStaticParams() {
  const posts = await fetch("https://api.example.com/posts").then((r) =>
    r.json()
  );

  return posts.map((post) => ({
    id: String(post.id),
  }));
}
```

**Server-Side Rendering (request time):**

```jsx
// Disable caching to fetch on every request
async function DashboardPage() {
  const data = await fetch("https://api.example.com/dashboard", {
    cache: "no-store", // Always fetch fresh
  }).then((r) => r.json());

  return <div>{/* render data */}</div>;
}
```

---

### **Lesson 10.5: API Routes**

**Create backend endpoints:**

```jsx
// app/api/posts/route.ts
import { NextResponse } from "next/server";

export async function GET() {
  const posts = [
    { id: 1, title: "First Post" },
    { id: 2, title: "Second Post" },
  ];

  return NextResponse.json(posts);
}

export async function POST(request) {
  const body = await request.json();

  // Save to database...

  return NextResponse.json({ success: true, data: body });
}
```

**Access at:** `http://localhost:3000/api/posts`

---

## **Module 11: TypeScript with React**

### **Lesson 11.1: Why TypeScript?**

**Benefits:**

- Catch errors before runtime
- Better IDE autocomplete
- Self-documenting code
- Refactoring confidence

**Setup:**

```bash
npm create vite@latest my-ts-app -- --template react-ts
```

---

### **Lesson 11.2: Component Types**

```tsx
import { useState } from "react";

// Props interface
interface UserCardProps {
  name: string;
  age: number;
  email?: string; // Optional
  onEdit: (id: number) => void;
}

function UserCard({ name, age, email, onEdit }: UserCardProps) {
  return (
    <div>
      <h3>{name}</h3>
      <p>Age: {age}</p>
      {email && <p>Email: {email}</p>}
      <button onClick={() => onEdit(1)}>Edit</button>
    </div>
  );
}
```

---

### **Lesson 11.3: Hooks with TypeScript**

```tsx
// useState
const [count, setCount] = useState<number>(0);
const [user, setUser] = useState<User | null>(null);

// Custom type
interface User {
  id: number;
  name: string;
  email: string;
}

const [users, setUsers] = useState<User[]>([]);

// useRef
const inputRef = useRef<HTMLInputElement>(null);

// useContext
interface ThemeContextType {
  theme: "light" | "dark";
  toggleTheme: () => void;
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined);
```

---

### **Lesson 11.4: Event Types**

```tsx
function Form() {
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value);
  };

  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
  };

  const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
    console.log("Clicked");
  };

  return (
    <form onSubmit={handleSubmit}>
      <input onChange={handleChange} />
      <button onClick={handleClick}>Submit</button>
    </form>
  );
}
```

---

## **Module 12: Testing React Applications**

### **Lesson 12.1: Testing Setup (Vitest + React Testing Library)**

```bash
npm install -D vitest @testing-library/react @testing-library/jest-dom @testing-library/user-event jsdom
```

**vite.config.js:**

```js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: "jsdom",
    setupFiles: "./src/test/setup.js",
  },
});
```

---

### **Lesson 12.2: Component Testing**

```jsx
// Button.test.jsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { describe, it, expect, vi } from "vitest";
import Button from "./Button";

describe("Button", () => {
  it("renders with correct text", () => {
    render(<Button>Click me</Button>);
    expect(screen.getByText("Click me")).toBeInTheDocument();
  });

  it("calls onClick when clicked", async () => {
    const handleClick = vi.fn();
    render(<Button onClick={handleClick}>Click</Button>);

    await userEvent.click(screen.getByText("Click"));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  it("is disabled when disabled prop is true", () => {
    render(<Button disabled>Click</Button>);
    expect(screen.getByText("Click")).toBeDisabled();
  });
});
```

---

### **Lesson 12.3: Testing Hooks**

```jsx
import { renderHook, act } from "@testing-library/react";
import { useCounter } from "./useCounter";

describe("useCounter", () => {
  it("increments counter", () => {
    const { result } = renderHook(() => useCounter(0));

    act(() => {
      result.current.increment();
    });

    expect(result.current.count).toBe(1);
  });
});
```

---

## **Module 13: Production Best Practices**

### **Lesson 13.1: Environment Variables**

```bash
# .env
VITE_API_URL=https://api.example.com
VITE_API_KEY=your-key-here
```

```jsx
const apiUrl = import.meta.env.VITE_API_URL;
```

**Rules:**

- Prefix with `VITE_` for Vite
- Never commit `.env` (add to `.gitignore`)
- Use `.env.production` for production values

---

### **Lesson 13.2: Build Optimization**

**Analyze bundle:**

```bash
npm install -D vite-plugin-bundle-analyzer
```

```js
// vite.config.js
import { visualizer } from "vite-plugin-bundle-analyzer";

export default defineConfig({
  plugins: [react(), visualizer()],
});
```

**Optimization checklist:**

- ✅ Code splitting (`React.lazy`)
- ✅ Tree shaking (remove unused code)
- ✅ Minification (automatic in production)
- ✅ Image optimization (WebP, lazy loading)
- ✅ CDN for static assets

---

### **Lesson 13.3: Error Handling**

```jsx
// Global error handler
function App() {
  useEffect(() => {
    window.addEventListener("error", (event) => {
      // Log to error tracking service (Sentry, etc.)
      console.error("Global error:", event.error);
    });
  }, []);

  return (
    <ErrorBoundary>
      <Router />
    </ErrorBoundary>
  );
}
```

---

## **Key Takeaways for Production Code**

1. **Component Design:** Small, focused, reusable
2. **State Management:** Local first, lift when needed, Redux for complex apps
3. **Performance:** Measure first, optimize bottlenecks, don't premature optimize[^1_3]
4. **Error Handling:** Error boundaries, try-catch for async, user-friendly messages
5. **Testing:** Test user behavior, not implementation details
6. **Accessibility:** Semantic HTML, ARIA labels, keyboard navigation
7. **Code Quality:** ESLint, Prettier, consistent naming, comments for "why" not "what"
8. **Modern Stack (2025):** Vite/Next.js + TypeScript + TanStack Query/Redux Toolkit + React Router[^1_1]

<div align="center">⁂</div>

---
