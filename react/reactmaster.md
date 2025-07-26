# React Mastery Guide for SDE2 Level

## Table of Contents
1. [Core React & Modern Hooks](#core-react--modern-hooks)
2. [State Management](#state-management)
3. [Component Design & Architecture](#component-design--architecture)
4. [Performance Optimization](#performance-optimization)
5. [Testing](#testing)

---

## Core React & Modern Hooks

### JSX, Components, and the Virtual DOM

#### What is JSX?
JSX is a syntax extension for JavaScript that looks like HTML but gets transpiled to `React.createElement()` calls.

```jsx
// This JSX...
const element = <h1 className="greeting">Hello, world!</h1>;

// ...gets transpiled to:
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

#### Virtual DOM Concept
The Virtual DOM is a JavaScript representation of the actual DOM. React uses it to optimize updates by comparing (diffing) the new virtual DOM tree with the previous one.

```jsx
// Before update
<div>
  <h1>Count: 0</h1>
  <button>Increment</button>
</div>

// After update (only the text node changes)
<div>
  <h1>Count: 1</h1>  // Only this text gets updated in the real DOM
  <button>Increment</button>
</div>
```

#### Real-Life Examples:
1. **Social Media Feed**: When you like a post, only the like button and count update, not the entire post
2. **Shopping Cart**: Adding items updates only the cart icon and total, not the whole page
3. **Chat Application**: New messages appear without refreshing the entire conversation
4. **Form Validation**: Error messages appear/disappear without reloading the form
5. **Dashboard Widgets**: Individual charts update with new data independently
6. **Search Results**: Typing filters results in real-time without page refresh
7. **Todo List**: Checking items updates only the specific list item

---

### Mastery of Hooks

#### useState: Managing Component State

```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('');

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      
      <input 
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Enter name"
      />
    </div>
  );
}
```

**Real-Life Examples:**
1. **Toggle Button**: Show/hide password visibility
2. **Form Input**: Managing user input in registration forms
3. **Shopping Cart**: Tracking number of items
4. **Modal State**: Open/close dialog boxes
5. **Loading States**: Showing spinners during API calls
6. **Theme Switcher**: Dark/light mode toggle
7. **Pagination**: Current page number

---

#### useEffect: Handling Side Effects

```jsx
import React, { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  // Effect with dependency array
  useEffect(() => {
    async function fetchUser() {
      setLoading(true);
      try {
        const response = await fetch(`/api/users/${userId}`);
        const userData = await response.json();
        setUser(userData);
      } catch (error) {
        console.error('Failed to fetch user:', error);
      } finally {
        setLoading(false);
      }
    }

    fetchUser();
  }, [userId]); // Only re-run when userId changes

  // Cleanup effect
  useEffect(() => {
    const interval = setInterval(() => {
      console.log('Checking for updates...');
    }, 5000);

    return () => clearInterval(interval); // Cleanup
  }, []);

  if (loading) return <div>Loading...</div>;
  
  return (
    <div>
      <h1>{user?.name}</h1>
      <p>{user?.email}</p>
    </div>
  );
}
```

**Real-Life Examples:**
1. **API Data Fetching**: Loading user profiles, product lists
2. **Event Listeners**: Window resize, scroll events
3. **Timers**: Auto-save drafts, session timeouts
4. **WebSocket Connections**: Real-time chat, live updates
5. **Document Title Updates**: Changing page title based on content
6. **Analytics Tracking**: Page views, user interactions
7. **Cleanup Operations**: Removing event listeners, canceling requests

---

#### useContext: Avoiding Prop Drilling

```jsx
import React, { createContext, useContext, useState } from 'react';

// Create context
const ThemeContext = createContext();

// Provider component
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// Custom hook for easier consumption
function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
}

// Component using the context
function Header() {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <header className={`header-${theme}`}>
      <h1>My App</h1>
      <button onClick={toggleTheme}>
        Switch to {theme === 'light' ? 'dark' : 'light'} mode
      </button>
    </header>
  );
}

// App component
function App() {
  return (
    <ThemeProvider>
      <Header />
      {/* Other components can also use theme without prop drilling */}
    </ThemeProvider>
  );
}
```

**Real-Life Examples:**
1. **Theme Management**: Dark/light mode across the app
2. **User Authentication**: Current user info accessible everywhere
3. **Language Settings**: Internationalization (i18n) context
4. **Shopping Cart**: Cart items accessible in header and checkout
5. **Notification System**: Global alerts and messages
6. **Feature Flags**: Enabling/disabling features across components
7. **Device Information**: Screen size, mobile detection

---

#### useReducer: Complex State Logic

```jsx
import React, { useReducer } from 'react';

// State shape
const initialState = {
  items: [],
  loading: false,
  error: null,
  filter: 'all'
};

// Action types
const actionTypes = {
  FETCH_START: 'FETCH_START',
  FETCH_SUCCESS: 'FETCH_SUCCESS',
  FETCH_ERROR: 'FETCH_ERROR',
  ADD_ITEM: 'ADD_ITEM',
  REMOVE_ITEM: 'REMOVE_ITEM',
  TOGGLE_ITEM: 'TOGGLE_ITEM',
  SET_FILTER: 'SET_FILTER'
};

// Reducer function
function todoReducer(state, action) {
  switch (action.type) {
    case actionTypes.FETCH_START:
      return { ...state, loading: true, error: null };
    
    case actionTypes.FETCH_SUCCESS:
      return { ...state, loading: false, items: action.payload };
    
    case actionTypes.FETCH_ERROR:
      return { ...state, loading: false, error: action.payload };
    
    case actionTypes.ADD_ITEM:
      return {
        ...state,
        items: [...state.items, {
          id: Date.now(),
          text: action.payload,
          completed: false
        }]
      };
    
    case actionTypes.TOGGLE_ITEM:
      return {
        ...state,
        items: state.items.map(item =>
          item.id === action.payload
            ? { ...item, completed: !item.completed }
            : item
        )
      };
    
    case actionTypes.SET_FILTER:
      return { ...state, filter: action.payload };
    
    default:
      return state;
  }
}

function TodoApp() {
  const [state, dispatch] = useReducer(todoReducer, initialState);

  const addTodo = (text) => {
    dispatch({ type: actionTypes.ADD_ITEM, payload: text });
  };

  const toggleTodo = (id) => {
    dispatch({ type: actionTypes.TOGGLE_ITEM, payload: id });
  };

  const filteredItems = state.items.filter(item => {
    if (state.filter === 'completed') return item.completed;
    if (state.filter === 'active') return !item.completed;
    return true;
  });

  return (
    <div>
      <h1>Todo App</h1>
      {/* Add todo form */}
      <TodoForm onAdd={addTodo} />
      
      {/* Filter buttons */}
      <FilterButtons 
        currentFilter={state.filter}
        onFilterChange={(filter) => 
          dispatch({ type: actionTypes.SET_FILTER, payload: filter })
        }
      />
      
      {/* Todo list */}
      <TodoList items={filteredItems} onToggle={toggleTodo} />
    </div>
  );
}
```

**Real-Life Examples:**
1. **Todo/Task Management**: Complex state with filters, sorting
2. **Shopping Cart**: Items, quantities, discounts, shipping
3. **Form Wizard**: Multi-step forms with validation
4. **Game State**: Score, level, player stats, inventory
5. **Chat Application**: Messages, users, typing indicators
6. **Data Table**: Sorting, filtering, pagination state
7. **Modal Management**: Multiple modals with different states

---

#### useRef: Direct DOM Access and Mutable Values

```jsx
import React, { useRef, useEffect, useState } from 'react';

function FocusInput() {
  const inputRef = useRef(null);
  const prevCountRef = useRef();
  const [count, setCount] = useState(0);

  // Focus input on mount
  useEffect(() => {
    inputRef.current.focus();
  }, []);

  // Store previous value
  useEffect(() => {
    prevCountRef.current = count;
  });

  const handleFocus = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} placeholder="I will be focused on mount" />
      <button onClick={handleFocus}>Focus Input</button>
      
      <div>
        <p>Current count: {count}</p>
        <p>Previous count: {prevCountRef.current}</p>
        <button onClick={() => setCount(c => c + 1)}>Increment</button>
      </div>
    </div>
  );
}

// Example: Measuring element dimensions
function MeasureDiv() {
  const divRef = useRef(null);
  const [dimensions, setDimensions] = useState({ width: 0, height: 0 });

  useEffect(() => {
    const updateDimensions = () => {
      if (divRef.current) {
        const { offsetWidth, offsetHeight } = divRef.current;
        setDimensions({ width: offsetWidth, height: offsetHeight });
      }
    };

    updateDimensions();
    window.addEventListener('resize', updateDimensions);
    
    return () => window.removeEventListener('resize', updateDimensions);
  }, []);

  return (
    <div ref={divRef} style={{ padding: '20px', border: '1px solid black' }}>
      <p>Width: {dimensions.width}px</p>
      <p>Height: {dimensions.height}px</p>
    </div>
  );
}
```

**Real-Life Examples:**
1. **Auto-focus Forms**: Focus first input field on page load
2. **Scroll to Element**: Navigate to specific sections
3. **File Upload**: Trigger hidden file input
4. **Video/Audio Controls**: Play, pause, seek operations
5. **Canvas Drawing**: Direct manipulation of canvas element
6. **Measuring Elements**: Get element dimensions for calculations
7. **Previous Values**: Comparing current vs previous state
8. **Animation Libraries**: Integrating with imperative animation APIs

---

### Custom Hooks: Reusable Logic

```jsx
// Custom hook for API data fetching
function useApi(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    async function fetchData() {
      try {
        setLoading(true);
        setError(null);
        const response = await fetch(url);
        if (!response.ok) throw new Error('Failed to fetch');
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    }

    fetchData();
  }, [url]);

  return { data, loading, error };
}

// Custom hook for local storage
function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error('Error reading localStorage:', error);
      return initialValue;
    }
  });

  const setValue = (value) => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error('Error setting localStorage:', error);
    }
  };

  return [storedValue, setValue];
}

// Custom hook for debounced values
function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);

  return debouncedValue;
}

// Usage examples
function UserList() {
  const { data: users, loading, error } = useApi('/api/users');
  const [theme, setTheme] = useLocalStorage('theme', 'light');
  const [searchTerm, setSearchTerm] = useState('');
  const debouncedSearchTerm = useDebounce(searchTerm, 300);

  // Filter users based on debounced search term
  const filteredUsers = users?.filter(user =>
    user.name.toLowerCase().includes(debouncedSearchTerm.toLowerCase())
  );

  if (loading) return <div>Loading users...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div className={`theme-${theme}`}>
      <button onClick={() => setTheme(t => t === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
      
      <input
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search users..."
      />

      <ul>
        {filteredUsers?.map(user => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

**Real-Life Examples of Custom Hooks:**
1. **useApi**: Fetching data from REST APIs
2. **useLocalStorage**: Persisting user preferences
3. **useDebounce**: Search input optimization
4. **useAuth**: Authentication state management
5. **useForm**: Form handling and validation
6. **useWindowSize**: Responsive design hooks
7. **useOnClickOutside**: Close modals when clicking outside
8. **useIntersectionObserver**: Lazy loading, infinite scroll

---

## State Management

### Client-Side vs. Server-Side State

#### Understanding the Difference

**Client-Side State**: Data that exists only in your application
- UI state (modals open/closed, form inputs)
- Navigation state (current page, sidebar collapsed)
- Temporary data (shopping cart before checkout)

**Server-Side State**: Data that comes from your backend
- User profiles, product catalogs
- Real-time data that can change on the server
- Needs synchronization between client and server

```jsx
// Client-side state example
function ProductPage() {
  const [isModalOpen, setIsModalOpen] = useState(false); // Client-side
  const [selectedTab, setSelectedTab] = useState('details'); // Client-side
  
  // Server-side state (should use proper data fetching)
  const [product, setProduct] = useState(null); // Server-side data
  
  return (
    <div>
      <button onClick={() => setIsModalOpen(true)}>
        Open Modal
      </button>
      
      <Tabs value={selectedTab} onChange={setSelectedTab}>
        <Tab value="details">Details</Tab>
        <Tab value="reviews">Reviews</Tab>
      </Tabs>
      
      {isModalOpen && (
        <Modal onClose={() => setIsModalOpen(false)}>
          Product details...
        </Modal>
      )}
    </div>
  );
}
```

**Real-Life Examples:**
1. **E-commerce**: Product data (server) vs cart UI state (client)
2. **Social Media**: Posts (server) vs like button animation (client)
3. **Dashboard**: Analytics data (server) vs chart type selector (client)
4. **Email Client**: Messages (server) vs sidebar collapsed (client)
5. **Online Editor**: Document content (server) vs cursor position (client)

---

### Global State Management

#### Redux Toolkit: Complex State Management

```jsx
// store/slices/userSlice.js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

// Async thunk for fetching user
export const fetchUser = createAsyncThunk(
  'user/fetchUser',
  async (userId, { rejectWithValue }) => {
    try {
      const response = await fetch(`/api/users/${userId}`);
      if (!response.ok) throw new Error('Failed to fetch');
      return await response.json();
    } catch (error) {
      return rejectWithValue(error.message);
    }
  }
);

const userSlice = createSlice({
  name: 'user',
  initialState: {
    data: null,
    loading: false,
    error: null
  },
  reducers: {
    clearUser: (state) => {
      state.data = null;
      state.error = null;
    },
    updateProfile: (state, action) => {
      if (state.data) {
        state.data = { ...state.data, ...action.payload };
      }
    }
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUser.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchUser.fulfilled, (state, action) => {
        state.loading = false;
        state.data = action.payload;
      })
      .addCase(fetchUser.rejected, (state, action) => {
        state.loading = false;
        state.error = action.payload;
      });
  }
});

export const { clearUser, updateProfile } = userSlice.actions;
export default userSlice.reducer;

// store/index.js
import { configureStore } from '@reduxjs/toolkit';
import userReducer from './slices/userSlice';

export const store = configureStore({
  reducer: {
    user: userReducer,
    // other reducers...
  },
});

// Component usage
import { useSelector, useDispatch } from 'react-redux';
import { fetchUser, updateProfile } from './store/slices/userSlice';

function UserProfile({ userId }) {
  const dispatch = useDispatch();
  const { data: user, loading, error } = useSelector(state => state.user);

  useEffect(() => {
    dispatch(fetchUser(userId));
  }, [dispatch, userId]);

  const handleUpdateProfile = (updates) => {
    dispatch(updateProfile(updates));
  };

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div>
      <h1>{user?.name}</h1>
      <button onClick={() => handleUpdateProfile({ name: 'New Name' })}>
        Update Name
      </button>
    </div>
  );
}
```

#### Zustand: Lightweight Alternative

```jsx
import { create } from 'zustand';
import { devtools, persist } from 'zustand/middleware';

// Simple store
const useStore = create(
  devtools(
    persist(
      (set, get) => ({
        // State
        count: 0,
        user: null,
        notifications: [],
        
        // Actions
        increment: () => set((state) => ({ count: state.count + 1 })),
        decrement: () => set((state) => ({ count: state.count - 1 })),
        
        setUser: (user) => set({ user }),
        
        addNotification: (notification) => set((state) => ({
          notifications: [...state.notifications, {
            id: Date.now(),
            ...notification
          }]
        })),
        
        removeNotification: (id) => set((state) => ({
          notifications: state.notifications.filter(n => n.id !== id)
        })),
        
        // Computed values
        get doubleCount() {
          return get().count * 2;
        }
      }),
      {
        name: 'app-storage', // localStorage key
        partialize: (state) => ({ count: state.count }), // Only persist count
      }
    )
  )
);

// Component usage
function Counter() {
  const count = useStore((state) => state.count);
  const increment = useStore((state) => state.increment);
  const doubleCount = useStore((state) => state.doubleCount);

  return (
    <div>
      <p>Count: {count}</p>
      <p>Double: {doubleCount}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}
```

**Real-Life Examples for Global State:**
1. **User Authentication**: Login status across the entire app
2. **Shopping Cart**: Items accessible from any page
3. **Theme Settings**: Dark/light mode for all components
4. **Notifications**: Global toast messages
5. **Language/Locale**: Internationalization settings
6. **Feature Flags**: Enable/disable features across app
7. **WebSocket Connection**: Real-time data updates

---

### Server-Side State & Caching: TanStack Query

```jsx
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';

// API functions
const api = {
  getUsers: async () => {
    const response = await fetch('/api/users');
    if (!response.ok) throw new Error('Failed to fetch users');
    return response.json();
  },
  
  getUser: async (id) => {
    const response = await fetch(`/api/users/${id}`);
    if (!response.ok) throw new Error('Failed to fetch user');
    return response.json();
  },
  
  updateUser: async ({ id, ...data }) => {
    const response = await fetch(`/api/users/${id}`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
    if (!response.ok) throw new Error('Failed to update user');
    return response.json();
  }
};

// Users list component
function UsersList() {
  const {
    data: users,
    isLoading,
    error,
    refetch
  } = useQuery({
    queryKey: ['users'],
    queryFn: api.getUsers,
    staleTime: 5 * 60 * 1000, // 5 minutes
    cacheTime: 10 * 60 * 1000, // 10 minutes
  });

  if (isLoading) return <div>Loading users...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <div>
      <button onClick={refetch}>Refresh</button>
      <ul>
        {users?.map(user => (
          <li key={user.id}>
            <UserItem userId={user.id} />
          </li>
        ))}
      </ul>
    </div>
  );
}

// Individual user component
function UserItem({ userId }) {
  const queryClient = useQueryClient();
  
  const { data: user } = useQuery({
    queryKey: ['user', userId],
    queryFn: () => api.getUser(userId),
    // Use cached data from users list if available
    initialData: () => {
      return queryClient.getQueryData(['users'])?.find(u => u.id === userId);
    }
  });

  const updateMutation = useMutation({
    mutationFn: api.updateUser,
    onSuccess: (updatedUser) => {
      // Update user in cache
      queryClient.setQueryData(['user', userId], updatedUser);
      
      // Update user in users list cache
      queryClient.setQueryData(['users'], (oldUsers) =>
        oldUsers?.map(u => u.id === userId ? updatedUser : u)
      );
    },
    onError: (error) => {
      console.error('Update failed:', error);
    }
  });

  const handleUpdate = () => {
    updateMutation.mutate({
      id: userId,
      name: 'Updated Name'
    });
  };

  return (
    <div>
      <span>{user?.name}</span>
      <button 
        onClick={handleUpdate}
        disabled={updateMutation.isLoading}
      >
        {updateMutation.isLoading ? 'Updating...' : 'Update'}
      </button>
    </div>
  );
}

// App setup with QueryClient
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retry: 3,
      staleTime: 1000 * 60 * 5, // 5 minutes
      cacheTime: 1000 * 60 * 10, // 10 minutes
    },
  },
});

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <UsersList />
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  );
}
```

**Real-Life Examples for TanStack Query:**
1. **Social Media Feed**: Cached posts with real-time updates
2. **E-commerce Products**: Product catalog with background refresh
3. **Dashboard Analytics**: Data that updates periodically
4. **User Profiles**: Cached user data across the app
5. **Chat Messages**: Optimistic updates for sent messages
6. **Search Results**: Cached search queries
7. **File Management**: Directory listings with cache invalidation

---

## Component Design & Architecture

### Component Patterns

#### Higher-Order Components (HOCs)

```jsx
// HOC for authentication
function withAuth(WrappedComponent) {
  return function AuthenticatedComponent(props) {
    const { user, loading } = useAuth();
    
    if (loading) return <div>Loading...</div>;
    if (!user) return <div>Please log in</div>;
    
    return <WrappedComponent {...props} user={user} />;
  };
}

// HOC for loading states
function withLoading(WrappedComponent) {
  return function LoadingComponent({ isLoading, ...props }) {
    if (isLoading) return <div>Loading...</div>;
    return <WrappedComponent {...props} />;
  };
}

// Usage
const ProtectedProfile = withAuth(withLoading(UserProfile));

function UserProfile({ user }) {
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
}
```

#### Render Props Pattern

```jsx
// Render props for mouse tracking
function MouseTracker({ children }) {
  const [mousePosition, setMousePosition] = useState({ x: 0, y: 0 });

  useEffect(() => {
    const handleMouseMove = (event) => {
      setMousePosition({ x: event.clientX, y: event.clientY });
    };

    window.addEventListener('mousemove', handleMouseMove);
    return () => window.removeEventListener('mousemove', handleMouseMove);
  }, []);

  return children(mousePosition);
}

// Data fetcher with render props
function DataFetcher({ url, children }) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    async function fetchData() {
      try {
        setLoading(true);
        const response = await fetch(url);
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err);
      } finally {
        setLoading(false);
      }
    }
    fetchData();
  }, [url]);

  return children({ data, loading, error });
}

// Usage
function App() {
  return (
    <div>
      <MouseTracker>
        {({ x, y }) => (
          <div>Mouse position: {x}, {y}</div>
        )}
      </MouseTracker>

      <DataFetcher url="/api/users">
        {({ data, loading, error }) => {
          if (loading) return <div>Loading...</div>;
          if (error) return <div>Error: {error.message}</div>;
          return (
            <ul>
              {data?.map(user => <li key={user.id}>{user.name}</li>)}
            </ul>
          );
        }}
      </DataFetcher>
    </div>
  );
}
```

#### Controlled vs. Uncontrolled Components

```jsx
// Controlled Component (React manages state)
function ControlledInput() {
  const [value, setValue] = useState('');
  const [email, setEmail] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Submitted:', { value, email });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        value={value}
        onChange={(e) => setValue(e.target.value)}
        placeholder="Controlled input"
      />
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
      />
      <button type="submit">Submit</button>
    </form>
  );
}

// Uncontrolled Component (DOM manages state)
function UncontrolledInput() {
  const nameRef = useRef();
  const emailRef = useRef();

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Submitted:', {
      name: nameRef.current.value,
      email: emailRef.current.value
    });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        ref={nameRef}
        defaultValue="Default name"
        placeholder="Uncontrolled input"
      />
      <input
        ref={emailRef}
        type="email"
        placeholder="Email"
      />
      <button type="submit">Submit</button>
    </form>
  );
}

// Hybrid approach - controlled with validation
function ValidatedForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    password: ''
  });
  const [errors, setErrors] = useState({});

  const validate = (field, value) => {
    const newErrors = { ...errors };
    
    switch (field) {
      case 'email':
        if (!/\S+@\S+\.\S+/.test(value)) {
          newErrors.email = 'Invalid email format';
        } else {
          delete newErrors.email;
        }
        break;
      case 'password':
        if (value.length < 8) {
          newErrors.password = 'Password must be at least 8 characters';
        } else {
          delete newErrors.password;
        }
        break;
      default:
        break;
    }
    
    setErrors(newErrors);
  };

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    validate(name, value);
  };

  return (
    <form>
      <div>
        <input
          name="name"
          value={formData.name}
          onChange={handleChange}
          placeholder="Name"
        />
      </div>
      
      <div>
        <input
          name="email"
          type="email"
          value={formData.email}
          onChange={handleChange}
          placeholder="Email"
        />
        {errors.email && <span className="error">{errors.email}</span>}
      </div>
      
      <div>
        <input
          name="password"
          type="password"
          value={formData.password}
          onChange={handleChange}
          placeholder="Password"
        />
        {errors.password && <span className="error">{errors.password}</span>}
      </div>
    </form>
  );
}
```

**Real-Life Examples:**
1. **Search Filters**: Controlled inputs for real-time filtering
2. **File Upload**: Uncontrolled for simple file selection
3. **Rich Text Editors**: Controlled for content management
4. **Quick Actions**: Uncontrolled for simple operations
5. **Multi-step Forms**: Controlled for validation and navigation
6. **Settings Panels**: Controlled for immediate feedback
7. **Bulk Operations**: Uncontrolled for performance

---

### Styling Strategies

#### CSS-in-JS with Styled-Components

```jsx
import styled, { createGlobalStyle, ThemeProvider } from 'styled-components';

// Global styles
const GlobalStyle = createGlobalStyle`
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  body {
    font-family: ${props => props.theme.fonts.primary};
    background-color: ${props => props.theme.colors.background};
    color: ${props => props.theme.colors.text};
  }
`;

// Theme
const theme = {
  colors: {
    primary: '#007bff',
    secondary: '#6c757d',
    success: '#28a745',
    danger: '#dc3545',
    background: '#ffffff',
    text: '#333333'
  },
  fonts: {
    primary: '-apple-system, BlinkMacSystemFont, sans-serif',
    monospace: 'Monaco, Consolas, monospace'
  },
  breakpoints: {
    mobile: '768px',
    tablet: '1024px'
  }
};

// Styled components
const Container = styled.div`
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
  
  @media (max-width: ${props => props.theme.breakpoints.mobile}) {
    padding: 0 10px;
  }
`;

const Button = styled.button`
  background-color: ${props => 
    props.variant === 'primary' ? props.theme.colors.primary :
    props.variant === 'danger' ? props.theme.colors.danger :
    props.theme.colors.secondary
  };
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: opacity 0.2s;
  
  &:hover {
    opacity: 0.8;
  }
  
  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
  
  ${props => props.size === 'large' && `
    padding: 16px 32px;
    font-size: 18px;
  `}
  
  ${props => props.fullWidth && `
    width: 100%;
  `}
`;

const Card = styled.div`
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 24px;
  margin-bottom: 16px;
  
  &:hover {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
  }
`;

// Usage
function App() {
  return (
    <ThemeProvider theme={theme}>
      <GlobalStyle />
      <Container>
        <Card>
          <h1>Welcome to the App</h1>
          <p>This is a styled component example.</p>
          <Button variant="primary" size="large">
            Get Started
          </Button>
          <Button variant="danger" fullWidth>
            Delete Account
          </Button>
        </Card>
      </Container>
    </ThemeProvider>
  );
}
```

#### Tailwind CSS: Utility-First Approach

```jsx
// Tailwind component examples
function TailwindComponents() {
  return (
    <div className="min-h-screen bg-gray-50">
      {/* Navigation */}
      <nav className="bg-white shadow-lg">
        <div className="max-w-7xl mx-auto px-4">
          <div className="flex justify-between items-center py-4">
            <div className="flex items-center space-x-4">
              <img className="h-8 w-8" src="/logo.svg" alt="Logo" />
              <h1 className="text-xl font-bold text-gray-800">My App</h1>
            </div>
            
            <div className="hidden md:flex space-x-4">
              <a href="#" className="text-gray-600 hover:text-gray-800 px-3 py-2 rounded-md text-sm font-medium">
                Home
              </a>
              <a href="#" className="text-gray-600 hover:text-gray-800 px-3 py-2 rounded-md text-sm font-medium">
                About
              </a>
              <a href="#" className="text-gray-600 hover:text-gray-800 px-3 py-2 rounded-md text-sm font-medium">
                Contact
              </a>
            </div>
            
            <button className="md:hidden">
              <svg className="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M4 6h16M4 12h16M4 18h16" />
              </svg>
            </button>
          </div>
        </div>
      </nav>

      {/* Hero Section */}
      <section className="bg-gradient-to-r from-blue-500 to-purple-600 text-white py-20">
        <div className="max-w-7xl mx-auto px-4 text-center">
          <h1 className="text-4xl md:text-6xl font-bold mb-6">
            Build Amazing Apps
          </h1>
          <p className="text-xl md:text-2xl mb-8 max-w-3xl mx-auto">
            Create beautiful, responsive applications with modern tools and best practices.
          </p>
          <div className="space-y-4 sm:space-y-0 sm:space-x-4 sm:flex sm:justify-center">
            <button className="w-full sm:w-auto bg-white text-blue-600 px-8 py-3 rounded-lg font-semibold hover:bg-gray-100 transition duration-200">
              Get Started
            </button>
            <button className="w-full sm:w-auto border-2 border-white text-white px-8 py-3 rounded-lg font-semibold hover:bg-white hover:text-blue-600 transition duration-200">
              Learn More
            </button>
          </div>
        </div>
      </section>

      {/* Cards Section */}
      <section className="py-16">
        <div className="max-w-7xl mx-auto px-4">
          <h2 className="text-3xl font-bold text-center mb-12">Features</h2>
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
            {[1, 2, 3, 4, 5, 6].map((item) => (
              <div key={item} className="bg-white rounded-lg shadow-md hover:shadow-xl transition-shadow duration-300 p-6">
                <div className="w-12 h-12 bg-blue-500 rounded-lg flex items-center justify-center mb-4">
                  <svg className="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M13 10V3L4 14h7v7l9-11h-7z" />
                  </svg>
                </div>
                <h3 className="text-xl font-semibold mb-2">Feature {item}</h3>
                <p className="text-gray-600 mb-4">
                  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt.
                </p>
                <a href="#" className="text-blue-500 hover:text-blue-600 font-medium inline-flex items-center">
                  Learn more
                  <svg className="w-4 h-4 ml-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M9 5l7 7-7 7" />
                  </svg>
                </a>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* Form Section */}
      <section className="bg-gray-100 py-16">
        <div className="max-w-md mx-auto bg-white rounded-lg shadow-md p-8">
          <h2 className="text-2xl font-bold mb-6 text-center">Contact Us</h2>
          <form className="space-y-4">
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Name
              </label>
              <input
                type="text"
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                placeholder="Your name"
              />
            </div>
            
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Email
              </label>
              <input
                type="email"
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                placeholder="your@email.com"
              />
            </div>
            
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Message
              </label>
              <textarea
                rows={4}
                className="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                placeholder="Your message..."
              />
            </div>
            
            <button
              type="submit"
              className="w-full bg-blue-500 text-white py-2 px-4 rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition duration-200"
            >
              Send Message
            </button>
          </form>
        </div>
      </section>
    </div>
  );
}
```

**Real-Life Examples for Styling:**
1. **Component Libraries**: Reusable button, card, input components
2. **Responsive Design**: Mobile-first layouts that work on all devices
3. **Theme Systems**: Dark/light mode switching
4. **Brand Consistency**: Maintaining design system across large apps
5. **Animation States**: Loading, hover, focus interactions
6. **Accessibility**: Focus indicators, screen reader support
7. **Print Styles**: Optimized layouts for printing

---

### Routing: React Router

```jsx
import { 
  BrowserRouter as Router,
  Routes,
  Route,
  Link,
  useNavigate,
  useParams,
  useLocation,
  Navigate,
  Outlet
} from 'react-router-dom';

// Protected Route component
function ProtectedRoute({ children }) {
  const { user } = useAuth();
  const location = useLocation();

  if (!user) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }

  return children;
}

// Layout component
function Layout() {
  const { user } = useAuth();
  
  return (
    <div className="app-layout">
      <nav>
        <Link to="/">Home</Link>
        <Link to="/products">Products</Link>
        {user ? (
          <>
            <Link to="/dashboard">Dashboard</Link>
            <Link to="/profile">Profile</Link>
          </>
        ) : (
          <Link to="/login">Login</Link>
        )}
      </nav>
      
      <main>
        <Outlet /> {/* Child routes render here */}
      </main>
    </div>
  );
}

// Product detail component with URL params
function ProductDetail() {
  const { productId } = useParams();
  const navigate = useNavigate();
  const [product, setProduct] = useState(null);

  useEffect(() => {
    // Fetch product data
    fetchProduct(productId).then(setProduct);
  }, [productId]);

  const handleEdit = () => {
    navigate(`/products/${productId}/edit`);
  };

  if (!product) return <div>Loading...</div>;

  return (
    <div>
      <button onClick={() => navigate('/products')}>← Back to Products</button>
      <h1>{product.name}</h1>
      <p>{product.description}</p>
      <button onClick={handleEdit}>Edit Product</button>
    </div>
  );
}

// Search results with query params
function SearchResults() {
  const location = useLocation();
  const navigate = useNavigate();
  const searchParams = new URLSearchParams(location.search);
  const query = searchParams.get('q') || '';
  const category = searchParams.get('category') || 'all';

  const updateSearch = (newQuery) => {
    const params = new URLSearchParams();
    if (newQuery) params.set('q', newQuery);
    if (category !== 'all') params.set('category', category);
    
    navigate(`/search?${params.toString()}`);
  };

  return (
    <div>
      <h1>Search Results for "{query}"</h1>
      <input
        value={query}
        onChange={(e) => updateSearch(e.target.value)}
        placeholder="Search products..."
      />
      {/* Results... */}
    </div>
  );
}

// Main App with routing
function App() {
  return (
    <Router>
      <Routes>
        {/* Public routes */}
        <Route path="/" element={<Layout />}>
          <Route index element={<Home />} />
          <Route path="login" element={<Login />} />
          <Route path="products" element={<ProductList />} />
          <Route path="products/:productId" element={<ProductDetail />} />
          <Route path="search" element={<SearchResults />} />
          
          {/* Protected routes */}
          <Route path="dashboard" element={
            <ProtectedRoute>
              <Dashboard />
            </ProtectedRoute>
          } />
          
          <Route path="profile" element={
            <ProtectedRoute>
              <Profile />
            </ProtectedRoute>
          } />
          
          {/* Nested routes for admin */}
          <Route path="admin" element={
            <ProtectedRoute>
              <AdminLayout />
            </ProtectedRoute>
          }>
            <Route index element={<AdminDashboard />} />
            <Route path="users" element={<UserManagement />} />
            <Route path="products" element={<ProductManagement />} />
          </Route>
          
          {/* Catch all route */}
          <Route path="*" element={<NotFound />} />
        </Route>
      </Routes>
    </Router>
  );
}

// Data loading with loaders (React Router v6.4+)
import { createBrowserRouter, RouterProvider } from 'react-router-dom';

const router = createBrowserRouter([
  {
    path: "/",
    element: <Layout />,
    children: [
      {
        path: "products/:productId",
        element: <ProductDetail />,
        loader: async ({ params }) => {
          return fetch(`/api/products/${params.productId}`);
        },
      },
    ],
  },
]);

function AppWithLoader() {
  return <RouterProvider router={router} />;
}
```

**Real-Life Examples for Routing:**
1. **E-commerce**: Product categories, detail pages, checkout flow
2. **Admin Dashboard**: User management, settings, analytics pages
3. **Blog/CMS**: Post listings, individual posts, author pages
4. **Social Media**: Profile pages, feed, messages, settings
5. **Documentation**: Nested sections, search results
6. **SaaS Applications**: Feature pages, billing, account management
7. **Multi-tenant Apps**: Organization-specific routes

---

## Performance Optimization

### Memoization Hooks

#### useMemo: Expensive Calculations

```jsx
import React, { useMemo, useState } from 'react';

function ExpensiveList({ items, searchTerm }) {
  // Expensive filtering and sorting - only recalculate when dependencies change
  const processedItems = useMemo(() => {
    console.log('Processing items...'); // This should only log when needed
    
    return items
      .filter(item => 
        item.name.toLowerCase().includes(searchTerm.toLowerCase())
      )
      .sort((a, b) => {
        // Expensive sorting algorithm
        return a.priority - b.priority;
      })
      .map(item => ({
        ...item,
        displayName: item.name.toUpperCase(), // Expensive transformation
        score: calculateComplexScore(item) // Expensive calculation
      }));
  }, [items, searchTerm]);

  return (
    <ul>
      {processedItems.map(item => (
        <li key={item.id}>
          {item.displayName} - Score: {item.score}
        </li>
      ))}
    </ul>
  );
}

// Complex calculation example
function UserStats({ users }) {
  const stats = useMemo(() => {
    const totalUsers = users.length;
    const activeUsers = users.filter(u => u.isActive).length;
    const averageAge = users.reduce((sum, u) => sum + u.age, 0) / totalUsers;
    const topUsers = users
      .sort((a, b) => b.score - a.score)
      .slice(0, 10);
    
    return {
      totalUsers,
      activeUsers,
      averageAge: Math.round(averageAge * 100) / 100,
      topUsers,
      activityRate: (activeUsers / totalUsers * 100).toFixed(1)
    };
  }, [users]);

  return (
    <div className="stats-dashboard">
      <div className="stat-card">
        <h3>Total Users</h3>
        <p>{stats.totalUsers}</p>
      </div>
      <div className="stat-card">
        <h3>Active Users</h3>
        <p>{stats.activeUsers} ({stats.activityRate}%)</p>
      </div>
      <div className="stat-card">
        <h3>Average Age</h3>
        <p>{stats.averageAge}</p>
      </div>
      <div className="stat-card">
        <h3>Top Performers</h3>
        <ul>
          {stats.topUsers.map(user => (
            <li key={user.id}>{user.name}</li>
          ))}
        </ul>
      </div>
    </div>
  );
}
```

#### useCallback: Function Memoization

```jsx
import React, { useCallback, useState, memo } from 'react';

// Child component that should only re-render when necessary
const TodoItem = memo(function TodoItem({ todo, onToggle, onDelete, onEdit }) {
  console.log(`Rendering TodoItem: ${todo.id}`);
  
  return (
    <div className="todo-item">
      <input
        type="checkbox"
        checked={todo.completed}
        onChange={() => onToggle(todo.id)}
      />
      <span className={todo.completed ? 'completed' : ''}>
        {todo.text}
      </span>
      <button onClick={() => onEdit(todo.id)}>Edit</button>
      <button onClick={() => onDelete(todo.id)}>Delete</button>
    </div>
  );
});

function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [filter, setFilter] = useState('all');

  // Memoized functions - won't cause child re-renders unless dependencies change
  const handleToggle = useCallback((id) => {
    setTodos(prevTodos =>
      prevTodos.map(todo =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    );
  }, []);

  const handleDelete = useCallback((id) => {
    setTodos(prevTodos => prevTodos.filter(todo => todo.id !== id));
  }, []);

  const handleEdit = useCallback((id) => {
    const newText = prompt('Edit todo:');
    if (newText) {
      setTodos(prevTodos =>
        prevTodos.map(todo =>
          todo.id === id ? { ...todo, text: newText } : todo
        )
      );
    }
  }, []);

  // Filter function also memoized
  const filteredTodos = useMemo(() => {
    switch (filter) {
      case 'active':
        return todos.filter(todo => !todo.completed);
      case 'completed':
        return todos.filter(todo => todo.completed);
      default:
        return todos;
    }
  }, [todos, filter]);

  return (
    <div>
      <div className="filters">
        <button 
          onClick={() => setFilter('all')}
          className={filter === 'all' ? 'active' : ''}
        >
          All
        </button>
        <button 
          onClick={() => setFilter('active')}
          className={filter === 'active' ? 'active' : ''}
        >
          Active
        </button>
        <button 
          onClick={() => setFilter('completed')}
          className={filter === 'completed' ? 'active' : ''}
        >
          Completed
        </button>
      </div>

      <div className="todo-list">
        {filteredTodos.map(todo => (
          <TodoItem
            key={todo.id}
            todo={todo}
            onToggle={handleToggle}
            onDelete={handleDelete}
            onEdit={handleEdit}
          />
        ))}
      </div>
    </div>
  );
}
```

#### React.memo: Component Memoization

```jsx
// Simple memo usage
const SimpleCard = memo(function SimpleCard({ title, content }) {
  console.log(`Rendering SimpleCard: ${title}`);
  return (
    <div className="card">
      <h3>{title}</h3>
      <p>{content}</p>
    </div>
  );
});

// Memo with custom comparison function
const ProductCard = memo(function ProductCard({ product, onAddToCart }) {
  return (
    <div className="product-card">
      <img src={product.image} alt={product.name} />
      <h3>{product.name}</h3>
      <p>${product.price}</p>
      <button onClick={() => onAddToCart(product.id)}>
        Add to Cart
      </button>
    </div>
  );
}, (prevProps, nextProps) => {
  // Custom comparison - only re-render if product data changed
  return (
    prevProps.product.id === nextProps.product.id &&
    prevProps.product.name === nextProps.product.name &&
    prevProps.product.price === nextProps.product.price &&
    prevProps.product.image === nextProps.product.image
  );
});

// Complex example with nested objects
const UserProfile = memo(function UserProfile({ user, settings, onUpdate }) {
  return (
    <div className="user-profile">
      <img src={user.avatar} alt={user.name} />
      <h2>{user.name}</h2>
      <p>{user.email}</p>
      
      {settings.showStats && (
        <div className="user-stats">
          <p>Posts: {user.stats.posts}</p>
          <p>Followers: {user.stats.followers}</p>
        </div>
      )}
      
      <button onClick={() => onUpdate(user.id)}>
        Update Profile
      </button>
    </div>
  );
}, (prevProps, nextProps) => {
  return (
    JSON.stringify(prevProps.user) === JSON.stringify(nextProps.user) &&
    JSON.stringify(prevProps.settings) === JSON.stringify(nextProps.settings)
  );
});
```

**Real-Life Examples for Memoization:**
1. **Data Tables**: Large datasets with filtering and sorting
2. **Search Results**: Expensive filtering operations
3. **Charts/Graphs**: Complex data calculations
4. **Form Validation**: Expensive validation rules
5. **Image Processing**: Filtering, transformations
6. **Shopping Cart**: Price calculations, tax computations
7. **Real-time Dashboards**: Data aggregation and statistics

---

### Code Splitting & Lazy Loading

```jsx
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';

// Lazy load components
const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
const Dashboard = lazy(() => 
  import('./pages/Dashboard').then(module => ({
    default: module.Dashboard
  }))
);

// Lazy load with delay for testing
const Products = lazy(() => 
  new Promise(resolve => {
    setTimeout(() => resolve(import('./pages/Products')), 1000);
  })
);

// Lazy load with error handling
const Admin = lazy(() => 
  import('./pages/Admin').catch(() => {
    return { default: () => <div>Failed to load Admin page</div> };
  })
);

// Loading component
function LoadingSpinner() {
  return (
    <div className="loading-container">
      <div className="spinner"></div>
      <p>Loading...</p>
    </div>
  );
}

// Error boundary for lazy components
class LazyErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    console.error('Lazy loading error:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return (
        <div className="error-container">
          <h2>Something went wrong</h2>
          <button onClick={() => this.setState({ hasError: false })}>
            Try Again
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}

// Main App with code splitting
function App() {
  return (
    <Router>
      <div className="app">
        <nav>
          <Link to="/">Home</Link>
          <Link to="/about">About</Link>
          <Link to="/products">Products</Link>
          <Link to="/dashboard">Dashboard</Link>
          <Link to="/admin">Admin</Link>
        </nav>

        <LazyErrorBoundary>
          <Suspense fallback={<LoadingSpinner />}>
            <Routes>
              <Route path="/" element={<Home />} />
              <Route path="/about" element={<About />} />
              <Route path="/products" element={<Products />} />
              <Route path="/dashboard" element={<Dashboard />} />
              <Route path="/admin" element={<Admin />} />
            </Routes>
          </Suspense>
        </LazyErrorBoundary>
      </div>
    </Router>
  );
}

// Advanced: Preloading components
function PreloadableLink({ to, children, preload = false }) {
  const [isPreloaded, setIsPreloaded] = useState(false);

  const preloadComponent = useCallback(() => {
    if (!isPreloaded) {
      switch (to) {
        case '/dashboard':
          import('./pages/Dashboard');
          break;
        case '/products':
          import('./pages/Products');
          break;
        // Add other cases...
      }
      setIsPreloaded(true);
    }
  }, [to, isPreloaded]);

  useEffect(() => {
    if (preload) {
      preloadComponent();
    }
  }, [preload, preloadComponent]);

  return (
    <Link
      to={to}
      onMouseEnter={preloadComponent}
      onFocus={preloadComponent}
    >
      {children}
    </Link>
  );
}

// Component-level code splitting
function FeatureModal({ isOpen, onClose }) {
  const [ModalContent, setModalContent] = useState(null);

  useEffect(() => {
    if (isOpen && !ModalContent) {
      // Only load the modal content when it's actually needed
      import('./components/AdvancedModalContent')
        .then(module => setModalContent(() => module.default))
        .catch(error => console.error('Failed to load modal:', error));
    }
  }, [isOpen, ModalContent]);

  if (!isOpen) return null;

  return (
    <div className="modal-overlay">
      <div className="modal">
        <button onClick={onClose}>Close</button>
        {ModalContent ? (
          <ModalContent />
        ) : (
          <div>Loading modal content...</div>
        )}
      </div>
    </div>
  );
}
```

**Real-Life Examples for Code Splitting:**
1. **Admin Panels**: Heavy admin features loaded only for admins
2. **Rich Text Editors**: Large editor libraries loaded on demand
3. **Charts/Visualization**: Heavy charting libraries split out
4. **Feature Modules**: Premium features loaded separately
5. **Third-party Integrations**: Payment, maps, chat widgets
6. **Locale/Language Packs**: Load translations on demand
7. **Device-specific Features**: Camera, geolocation components

---

### List Virtualization

```jsx
import React, { useMemo, useState } from 'react';
import { FixedSizeList as List } from 'react-window';

// Simple virtualized list
function VirtualizedList({ items }) {
  const Row = ({ index, style }) => (
    <div style={style} className="list-item">
      <div className="item-content">
        <h3>{items[index].name}</h3>
        <p>{items[index].description}</p>
        <span className="item-id">ID: {items[index].id}</span>
      </div>
    </div>
  );

  return (
    <List
      height={600} // Container height
      itemCount={items.length}
      itemSize={120} // Height of each item
      width="100%"
    >
      {Row}
    </List>
  );
}

// Advanced virtualized table
import { VariableSizeList as VariableList } from 'react-window';

function VirtualizedTable({ data, columns }) {
  const [sortConfig, setSortConfig] = useState({ key: null, direction: 'asc' });

  // Sorted data
  const sortedData = useMemo(() => {
    if (!sortConfig.key) return data;

    return [...data].sort((a, b) => {
      if (a[sortConfig.key] < b[sortConfig.key]) {
        return sortConfig.direction === 'asc' ? -1 : 1;
      }
      if (a[sortConfig.key] > b[sortConfig.key]) {
        return sortConfig.direction === 'asc' ? 1 : -1;
      }
      return 0;
    });
  }, [data, sortConfig]);

  const handleSort = (key) => {
    setSortConfig(prevConfig => ({
      key,
      direction: prevConfig.key === key && prevConfig.direction === 'asc' ? 'desc' : 'asc'
    }));
  };

  // Calculate item height based on content
  const getItemSize = (index) => {
    const item = sortedData[index];
    // Base height + additional height for longer descriptions
    return 60 + Math.floor(item.description.length / 50) * 20;
  };

  const Row = ({ index, style }) => {
    const item = sortedData[index];
    
    return (
      <div style={style} className="table-row">
        {columns.map((column) => (
          <div key={column.key} className="table-cell" style={{ width: column.width }}>
            {column.render ? column.render(item[column.key], item) : item[column.key]}
          </div>
        ))}
      </div>
    );
  };

  return (
    <div className="virtual-table">
      {/* Table Header */}
      <div className="table-header">
        {columns.map((column) => (
          <div
            key={column.key}
            className="table-header-cell"
            style={{ width: column.width }}
            onClick={() => handleSort(column.key)}
          >
            {column.title}
            {sortConfig.key === column.key && (
              <span>{sortConfig.direction === 'asc' ? ' ↑' : ' ↓'}</span>
            )}
          </div>
        ))}
      </div>

      {/* Virtualized Body */}
      <VariableList
        height={500}
        itemCount={sortedData.length}
        itemSize={getItemSize}
        width="100%"
      >
        {Row}
      </VariableList>
    </div>
  );
}

// Infinite loading with virtualization
function InfiniteVirtualList() {
  const [items, setItems] = useState([]);
  const [hasNextPage, setHasNextPage] = useState(true);
  const [isLoading, setIsLoading] = useState(false);

  const loadMoreItems = async () => {
    if (isLoading) return;
    
    setIsLoading(true);
    try {
      const response = await fetch(`/api/items?offset=${items.length}&limit=50`);
      const newItems = await response.json();
      
      setItems(prevItems => [...prevItems, ...newItems]);
      setHasNextPage(newItems.length === 50);
    } catch (error) {
      console.error('Failed to load items:', error);
    } finally {
      setIsLoading(false);
    }
  };

  // Load initial items
  useEffect(() => {
    loadMoreItems();
  }, []);

  const Row = ({ index, style }) => {
    const item = items[index];
    
    // Load more items when near the end
    if (!isLoading && hasNextPage && index === items.length - 10) {
      loadMoreItems();
    }

    // Show loading placeholder for items that haven't loaded yet
    if (!item) {
      return (
        <div style={style} className="loading-item">
          <div className="skeleton-content">
            <div className="skeleton-title"></div>
            <div className="skeleton-text"></div>
          </div>
        </div>
      );
    }

    return (
      <div style={style} className="list-item">
        <h3>{item.title}</h3>
        <p>{item.description}</p>
        <div className="item-meta">
          <span>By {item.author}</span>
          <span>{new Date(item.createdAt).toLocaleDateString()}</span>
        </div>
      </div>
    );
  };

  return (
    <div className="infinite-list-container">
      <h1>Infinite Scroll List ({items.length} items)</h1>
      <List
        height={600}
        itemCount={hasNextPage ? items.length + 10 : items.length} // Extra items for loading
        itemSize={150}
        width="100%"
      >
        {Row}
      </List>
      {isLoading && (
        <div className="loading-indicator">Loading more items...</div>
      )}
    </div>
  );
}

// Usage example
function App() {
  const [selectedView, setSelectedView] = useState('simple');
  
  // Generate large dataset for testing
  const largeDataset = useMemo(() => 
    Array.from({ length: 10000 }, (_, i) => ({
      id: i,
      name: `Item ${i}`,
      description: `This is a description for item ${i}. It contains some text that might be longer or shorter.`,
      author: `Author ${i % 100}`,
      createdAt: new Date(Date.now() - Math.random() * 10000000000).toISOString()
    }))
  , []);

  const tableColumns = [
    { key: 'id', title: 'ID', width: '10%' },
    { key: 'name', title: 'Name', width: '30%' },
    { key: 'author', title: 'Author', width: '20%' },
    { 
      key: 'createdAt', 
      title: 'Created', 
      width: '20%',
      render: (value) => new Date(value).toLocaleDateString()
    },
    { key: 'description', title: 'Description', width: '20%' }
  ];

  return (
    <div className="app">
      <div className="controls">
        <button 
          onClick={() => setSelectedView('simple')}
          className={selectedView === 'simple' ? 'active' : ''}
        >
          Simple List
        </button>
        <button 
          onClick={() => setSelectedView('table')}
          className={selectedView === 'table' ? 'active' : ''}
        >
          Table View
        </button>
        <button 
          onClick={() => setSelectedView('infinite')}
          className={selectedView === 'infinite' ? 'active' : ''}
        >
          Infinite Scroll
        </button>
      </div>

      {selectedView === 'simple' && <VirtualizedList items={largeDataset} />}
      {selectedView === 'table' && <VirtualizedTable data={largeDataset} columns={tableColumns} />}
      {selectedView === 'infinite' && <InfiniteVirtualList />}
    </div>
  );
}
```

**Real-Life Examples for List Virtualization:**
1. **Data Tables**: Large datasets with thousands of rows
2. **Chat Applications**: Message history with thousands of messages
3. **Social Media Feeds**: Infinite scroll feeds
4. **File Explorers**: Directories with many files
5. **Product Catalogs**: E-commerce listings
6. **Log Viewers**: Application logs with many entries
7. **Analytics Dashboards**: Large datasets visualization

---

## Testing

### Testing Philosophy

#### Focus on Behavior, Not Implementation

```jsx
// ❌ Bad: Testing implementation details
test('should update state when input changes', () => {
  const { getByRole } = render(<LoginForm />);
  const input = getByRole('textbox', { name: /username/i });
  
  fireEvent.change(input, { target: { value: 'john' } });
  
  // This tests internal state, not user-visible behavior
  expect(component.state.username).toBe('john');
});

// ✅ Good: Testing user-visible behavior
test('should enable submit button when form is valid', () => {
  const { getByRole } = render(<LoginForm />);
  const usernameInput = getByRole('textbox', { name: /username/i });
  const passwordInput = getByLabelText(/password/i);
  const submitButton = getByRole('button', { name: /log in/i });
  
  // Initially disabled
  expect(submitButton).toBeDisabled();
  
  // Fill out form
  userEvent.type(usernameInput, 'john@example.com');
  userEvent.type(passwordInput, 'password123');
  
  // Should be enabled when valid
  expect(submitButton).toBeEnabled();
});
```

#### Component Testing Examples

```jsx
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { UserProfile } from './UserProfile';

describe('UserProfile', () => {
  const mockUser = {
    id: 1,
    name: 'John Doe',
    email: 'john@example.com',
    avatar: 'https://example.com/avatar.jpg'
  };

  test('displays user information correctly', () => {
    render(<UserProfile user={mockUser} />);
    
    expect(screen.getByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('john@example.com')).toBeInTheDocument();
    expect(screen.getByRole('img', { name: /john doe/i })).toHaveAttribute(
      'src', 
      'https://example.com/avatar.jpg'
    );
  });

  test('handles edit mode correctly', async () => {
    const user = userEvent.setup();
    const onUpdate = jest.fn();
    
    render(<UserProfile user={mockUser} onUpdate={onUpdate} />);
    
    // Enter edit mode
    await user.click(screen.getByRole('button', { name: /edit/i }));
    
    // Form should appear
    expect(screen.getByRole('textbox', { name: /name/i })).toBeInTheDocument();
    expect(screen.getByRole('textbox', { name: /email/i })).toBeInTheDocument();
    
    // Update name
    const nameInput = screen.getByRole('textbox', { name: /name/i });
    await user.clear(nameInput);
    await user.type(nameInput, 'Jane Doe');
    
    // Save changes
    await user.click(screen.getByRole('button', { name: /save/i }));
    
    expect(onUpdate).toHaveBeenCalledWith({
      ...mockUser,
      name: 'Jane Doe'
    });
  });

  test('validates email format', async () => {
    const user = userEvent.setup();
    render(<UserProfile user={mockUser} onUpdate={jest.fn()} />);
    
    await user.click(screen.getByRole('button', { name: /edit/i }));
    
    const emailInput = screen.getByRole('textbox', { name: /email/i });
    await user.clear(emailInput);
    await user.type(emailInput, 'invalid-email');
    
    // Try to save
    await user.click(screen.getByRole('button', { name: /save/i }));
    
    expect(screen.getByText(/please enter a valid email/i)).toBeInTheDocument();
  });
});

// Form testing
describe('ContactForm', () => {
  test('submits form with correct data', async () => {
    const user = userEvent.setup();
    const onSubmit = jest.fn();
    
    render(<ContactForm onSubmit={onSubmit} />);
    
    // Fill out form
    await user.type(screen.getByRole('textbox', { name: /name/i }), 'John Doe');
    await user.type(screen.getByRole('