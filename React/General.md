## `useState`
- if you need to rely on the previous value it is best practice to use the callback version of `useState` otherwise you can set a direct value
```jsx
const [count, setCount] = useState(0);

setCount(0);
setCount((prevCount) => prevCount + 1);
```

## typescript
- typescript will infer the type, however, sometimes it might be necessary to specify your type (example: `useState<UserType | null>(null)`)
```tsx
const [count, setCount] = useState<number>(0);

function ResetButton(setCount: React.Dispatch<React.SetStateAction<number>>) {
	const handleClick = () => setCount(0);
	return (<button onClick={handleClick}>Reset</button>);
}
```

## initializer function
- react will only call the initializer function during its first render.
```jsx
 const [items, setItems] = useState(
    () => JSON.parse(localStorage.getItem("items")) ?? initialItems
  );
```


---

## `useEffect`
- returning a function in a `useEffect` acts a cleanup function so that you can remove a previous side effect before adding it again.
```tsx
  useEffect(() => {
    const handleKeyDown = (event: KeyboardEvent) => {
      if (event.code === "Space") {
        setCount((prev) => prev + 1);
      }
    };
    window.addEventListener("keydown", handleKeyDown);

    return () => {
      window.removeEventListener("keydown", handleKeyDown);
    };
  }, [count]);
```

--- 
## `useMemo`
- will only run when `items` and `sortBy` change (as well as on initial render)
- improves optimization for expensive operations such as sorting
```jsx
const sortedItems = useMemo(() => [...items].sort((a, b) => {
	switch (sortBy) {
		case "packed":
			return b.packed - a.packed;
		case "unpacked":
			return a.packed - b.packed;
		default:
			return;
	}
}), [items, sortBy] );
```

---

## `Create Context`
```jsx
import { createContext, useEffect, useState } from "react";
import { initial } from "../lib/constants";

export const ItemsContext = createContext();

export default function ItemsContextProvider({ children }) {
  const [items, setItems] = useState(() => {
	  return JSON.parse(localStorage.getItem("items")) ?? initial
  });

  const handleAddItem = (newItemText) => { /* ... */  };
  const handleToggleItem = (id) => { /* ... */ };

  useEffect(() => {
    localStorage.setItem("items", JSON.stringify(items));
  }, [items]);

  const exposed = { items, handleAddItem, handleToggleItem,}

  return (
    <ItemsContext.Provider value={exposed}>
      {children}
    </ItemsContext.Provider>
  );
}
```

----
## `useContext` (create custom hook)
```jsx
import { useContext } from "react";
import { ItemsContext } from "../contexts/ItemsContextProvider";

export function useItemsContext() {
  const context = useContext(ItemsContext);

  if (!context) {
    throw new Error(
      "useItemsContext must be used within an ItemsContextProvider"
    );
  }

  return context;
}
```

## Caveat of `Context API`
- when using the context API, any component that is accessing the context will re-render if any of the exposed values or methods changes even if that specific component is not using the changed valued value or methods
- 
```jsx
import { createContext, useEffect, useState } from "react";
import { initial } from "../lib/constants";

export const ItemsContext = createContext();

export default function ItemsContextProvider({ children }) {
  const [items, setItems] = useState(initial);
  const handleAddItem = (newItemText) => { /* ... */  };

  const exposed = { items, handleAddItem,}

  return (
    <ItemsContext.Provider value={exposed}>
      {children}
    </ItemsContext.Provider>
  );
}
```

```jsx
import AddItemForm from "./AddItemForm";
import { useItemsContext } from "../lib/hooks";

export default function Sidebar() {
  const { handleAddItem } = useItemsContext();
  return (<AddItemForm onAddItem={handleAddItem} />);
}
```

> here, any time the items change, the `Sidebar Component` will re-render even though it is only accessing the `handleAddItem` function of our items context

---
## `Zustand` Example
- using a `zustand` store will prevent unnecessary re-renders. It will only re-render for components that subscribe to the specific piece of state that has changed. (`useMemo` under the hood)
```jsx
import { create } from "zustand";
import { persist } from "zustand/middleware";
import { initialItems } from "../lib/constants";

export const useItemStore = create(
  persist((set) => ({
      items: initialItems,
      addItem: (newItemText) => {
        const newItem = { 
        id: Date.now(), name: newItemText, packed: false 
        };

        set((state) => ({ items: [...state.items, newItem] }));
      },
          return { items: newItems };
	}), { name: "items" })
);
```




---

## Children Composition
```jsx
<ButtonContainer>
	<CountButton type="minus" setCount={setCount} locked={locked} />
	<CountButton type="plus" setCount={setCount} locked={locked} />
</ButtonContainer>
```

```jsx
export default function ButtonContainer({ children }) {
  return <div className="button-container">{children}</div>;
}
```


## `AbortController`
```tsx
import { useRef } from "react";
const controllerRef = useRef<AbortController>();

const handleChange = (e: React.SyntheticEvent) => {
	if ( controllerRef.current ) {
		controllerRef.current.abort();
	}

	controllerRef.current = new AbortControlelr();
	const signal = controllerRef.current.signal;

	try {	
		const response = await fetch("/api/search", {
			signal
		};

		const json = await response.json();
	} catch (error) {
		console.log(error);
	}
}
```











---


==example== remove button focus
```tsx
const handleClick = (event: React.MouseEvent<HTMLButtonElement, MouseEvent>) => {
	setCount(0);
	event.currentTarget.blur();
}

<button onClick={handleClick}>Reset</button>
```


==example== count words in a string
```javascript
const numberOfWords = text.split(/\s/)
	.filter((word) => word !== "").length;
```