1. What is React?

React is a JavaScript library developed by Meta for building user interfaces using reusable components. 
It uses a Virtual DOM to efficiently update the UI.

2. What is the Virtual DOM?

The Virtual DOM is a lightweight copy of the real DOM.

Process:

React creates a Virtual DOM tree.
When state changes, React creates a new Virtual DOM.
React compares (diffs) the old and new Virtual DOM.
Only changed elements are updated in the real DOM.

This improves performance.

3. What is JSX?

JSX (JavaScript XML) allows writing HTML-like syntax inside JavaScript.

const element = <h1>Hello React</h1>;

JSX is transpiled into:

React.createElement("h1", null, "Hello React");

4. What are Functional Components?

Functional components are JavaScript functions that return JSX.

Benefits:
  - Simpler syntax
  - Hooks support
  - Better readability

Ex.,

function Welcome() {
  return <h1>Hello</h1>;
}

5. What is the difference between Functional and Class Components?

Functional	Class
Uses Hooks	Uses Lifecycle Methods
Less boilerplate	More boilerplate
Preferred in modern React	Legacy approach
Easier to test	More complex


6. What are React Hooks?

Hooks allow functional components to use React features like state and lifecycle methods.

Common Hooks:
 - useState
 - useEffect
 - useContext
 - useMemo
 - useCallback
 - useReducer
 - useRef


7. Explain useState.

const [count, setCount] = useState(0);
count = current state
setCount = updates state
setCount(count + 1);

8. Explain useEffect.

Used for side effects:

API calls
Event listeners
Timers
DOM updates
useEffect(() => {
  fetchUsers();
}, []);

Dependency array:

[] → runs once
[id] → runs when id changes
No dependency → runs every render

9. Difference between useEffect and useLayoutEffect?

useEffect	useLayoutEffect
Runs after paint	Runs before paint
Non-blocking	Blocking
Used for API calls	Used for DOM measurements

10. What is Prop Drilling?

Passing props through multiple components.

App → Parent → Child → GrandChild

Problem:

Difficult maintenance

Solutions:

Context API
Redux


11. What is Context API?

Used to share data globally without prop drilling.

const UserContext = createContext();

Provider:

<UserContext.Provider value={user}>

Consumer:

const user = useContext(UserContext);

12. What is Redux?

Redux is a state management library used for managing global application state.

Main concepts:
  - Store
  - Actions
  - Reducers
  - Dispatch

Flow:

Component
   ↓
Dispatch Action
   ↓
Reducer
   ↓
Store Updated
   ↓
UI Re-render

13. Difference Between Redux and Context API?

Redux	Context API
Large apps	Small-medium apps
Middleware support	No middleware
Better debugging	Simpler
Predictable state	Less structured

14. What is useMemo?

Used to memoize expensive calculations.

const total = useMemo(() => {
  return calculateTotal(items);
}, [items]);

Prevents unnecessary recalculations.

15. What is useCallback?

Memoizes functions.

const handleClick = useCallback(() => {
  saveData();
}, []);

Useful when passing functions to child components.

16. Difference Between useMemo and useCallback?

useMemo	useCallback
Memoizes value	Memoizes function
Returns value	Returns function

17. What is React.memo?

Prevents unnecessary component re-renders.

export default React.memo(UserCard);

Re-renders only when props change.

18. What is useRef?

Stores mutable values without re-rendering.

const inputRef = useRef();

Access DOM:

inputRef.current.focus();

19. What is Reconciliation?

The process React uses to compare Virtual DOM trees and determine the minimum changes required for the real DOM.

20. Why are Keys Important in Lists?

Answer:

users.map(user => (
  <li key={user.id}>{user.name}</li>
))

Keys help React identify changed, added, or removed items efficiently.

21. What is Code Splitting?

Answer:

Loads code only when required.

const Dashboard = React.lazy(() => import('./Dashboard'));

Used with:

<Suspense fallback={<Loader />}>

Improves initial page load.

22. What is Lazy Loading?

Answer:

Loading components on demand rather than loading the entire application at startup.

23. How do you optimize React performance?

Answer:

React.memo
useMemo
useCallback
Code Splitting
Lazy Loading
Pagination
Virtualization
Avoid unnecessary re-renders
Proper key usage
24. What is Higher Order Component (HOC)?

Answer:

A function that takes a component and returns a new component.

const withAuth = (Component) => {
  return (props) => {
    return <Component {...props} />;
  };
};
25. What are Custom Hooks?

Answer:

Reusable hook logic.

function useFetch(url) {
  // logic
}

Used across multiple components.

26. Explain React Router.

Answer:

Used for client-side routing.

<Route path="/users" element={<Users />} />

Common APIs:

BrowserRouter
Routes
Route
Navigate
useNavigate
useParams

27. Difference Between Controlled and Uncontrolled Components?
Controlled
<input value={name} onChange={handleChange} />

State managed by React.

Uncontrolled
<input ref={inputRef} />

State managed by DOM.

28. How do you call APIs in React?

Answer:

Using fetch or libraries such as Axios.

useEffect(() => {
  fetch('/api/users')
    .then(res => res.json())
    .then(setUsers);
}, []);
29. What is the difference between State and Props?
State	Props
Mutable	Immutable
Managed inside component	Passed from parent
Can change	Read-only
30. What React questions are commonly asked based on real projects?
API Integration
How do you handle API failures?
How do you implement retries?
How do you cancel API requests?
State Management
Why did you choose Redux over Context API?
How do you structure Redux slices?
Performance
How did you reduce page load time?
Have you used lazy loading?
Testing
Have you used Jest?
Have you used React Testing Library?
Architecture
How do you organize a large React application?
How do you manage reusable components?
