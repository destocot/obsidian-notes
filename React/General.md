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


## `useEffect` / `useRef` to close popover
```tsx
import { forwardRef } from "react";
import { useBookmarksContext } from "../lib/hooks";
import JobList from "./JobList";

type BookmarksPopoverType = unknown;

const BookmarksPopover = forwardRef<HTMLDivElement, BookmarksPopoverType>(
  function (_, ref) {
    const { bookmarkedJobItems, isLoading } = useBookmarksContext();

    return (
      <div ref={ref} className="bookmarks-popover">
        <JobList jobItems={bookmarkedJobItems} isLoading={isLoading} />
      </div>
    );
  }
);

export default BookmarksPopover;
```

```tsx
import { TriangleDownIcon } from "@radix-ui/react-icons";
import BookmarksPopover from "./BookmarksPopover";
import { useEffect, useRef, useState } from "react";

export default function BookmarksButton() {
  const [isOpen, setIsOpen] = useState(false);
  const buttonRef = useRef<HTMLButtonElement>(null);
  const popoverRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const handleClick = (e: MouseEvent) => {
      if (
        e.target instanceof HTMLElement &&
        !buttonRef.current?.contains(e.target) &&
        !popoverRef.current?.contains(e.target)
      ) {
        setIsOpen(false);
      }
    };
    document.addEventListener("click", handleClick);

    return () => {
      document.removeEventListener("click", handleClick);
    };
  }, []);
  return (
    <section>
      <button
        ref={buttonRef}
        className="bookmarks-btn"
        onClick={() => setIsOpen((prev) => !prev)}
      >
        Bookmarks <TriangleDownIcon />
      </button>
      {isOpen && <BookmarksPopover ref={popoverRef} />}
    </section>
  );
}
```
> alternatively can use the following `handleClick` function which relies on class names

```tsx
const handleClick = (e: MouseEvent) => {
      if (
        e.target instanceof HTMLElement &&
        !e.target.closest(".bookmarks-btn") &&
        !e.target.closest(".bookmarks-popover")
      ) {
        setIsOpen(false);
      }
    };
```
- see custom hook variation of this below

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

```tsx
  const companyList = useMemo(() =>
      feedbackItems
        .map((item) => item.company)
        .filter((company, index, array) => {
          return array.indexOf(company) === index;
        }), [feedbackItems]);
```

**can memorize context object**
```tsx
  const exposed: JobItemsContext = useMemo(
    () => ({
      jobItems, jobItemsSortedAndSliced, isLoading, totalNumberOfResults,
      totalNumberOfPages, currentPage, sortBy, handleChangePage,
      handleChangeSortBy,
    }),
    [
      jobItems, jobItemsSortedAndSliced, isLoading, totalNumberOfResults,
      totalNumberOfPages, currentPage, sortBy, handleChangePage,
      handleChangeSortBy,
    ]
  );
```

---
# `useCallback`
- to `memoize` function
```tsx
  const handleChangePage = useCallback((direction: PageDirection) => {
    if (direction === "next") {
      setCurrentPage((prev) => prev + 1);
    } else if (direction === "previous") {
      setCurrentPage((prev) => prev - 1);
    }
    // console.log(currentPage);
  }, []);

  const handleChangeSortBy = useCallback((newSortBy: SortBy) => {
    setCurrentPage(1);
    setSortBy(newSortBy);
  }, []);
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
- by throwing an error we inform the developer they are not using the context in the correct place, furthermore, it prevents the null check when the context is used.

> `useContext` can be used to share a single value around your application, this can be powerful for performance so that one is not constantly creating event listeners or state extraneously

- refactoring with `useContext` can optimize unnecessary re-renders
- prevents prop drilling since we can pull values directly out of the context
- you can consume contexts in other contexts (it's just a component after all)
	- as long as the context that is consuming another context is nested within

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
## Custom Hook (Semantically Group)
```tsx
import { useContext, useEffect, useState } from "react";

export type TFeedbackItem = { id: number; company: string; text: string };
const BASE_URL = "https://bytegrad.com/course-assets/projects/corpcomment"
```

```tsx
export function useFeedbackItems() {
  const [feedbackItems, setFeedbackItems] = useState<TFeedbackItem[]>([]);
  const [isLoading, setIsLoading] = useState(false);
  const [errorMessage, setErrorMessage] = useState("");


  useEffect(() => {
    (async function run() {
      setIsLoading(true);
      try {
        const response = await fetch(`${BASE_URL}/api/feedbacks`);
        if (!response.ok) throw new Error();

        const data = await response.json();
        setFeedbackItems(data.feedbacks);
      } catch {
        setErrorMessage("Something went wrong.");
      }
      setIsLoading(false);
    })();
  }, []);

  return { isLoading, errorMessage, feedbackItems, setFeedbackItems };
}
```
- **returning items from a custom hook as an array has the following advantage**
	- it will allow you to rename the variables whatever you would like upon destructuring
	- however, typescript will complain about the types, to  make the type more specific we can do the following
```tsx
export function useJobItems(searchText: string) {
	/* ... */
  return [jobItemsSliced, isLoading] as const;
}
```

## Custom Debounce With Generics
```tsx
export function useDebounce<T>(value: T, delay = 500): T {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timerId = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);
    return () => clearTimeout(timerId);
  }, [value, delay]);

  return debouncedValue;
}
```

- arrow function
```tsx
export const useDebounce = <T>(value: T, delay = 500): T => { /*...*/ }
```

---
## Custom `useLocalStorage` With Generics
```tsx
export function useLocalStorage<T>(
  key: string,
  initialValue: T
): [T, React.Dispatch<React.SetStateAction<T>>] {
  const [value, setValue] = useState(() =>
    JSON.parse(localStorage.getItem(key) ?? JSON.stringify(initialValue))
  );

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [value, key]);

  return [value, setValue] as const;
}
```

```tsx
const [bookmarkedIds, setBookmarkedIds] = useLocalStorage<number[]>(
    "bookmarkedIds", [] );
```

---

# Custom `useOnClickOutside`
- implementation
```tsx
export function useOnClickOutside(
  refs: React.RefObject<HTMLElement>[],
  handler: () => void
) {
  useEffect(() => {
    const handleClick = (e: MouseEvent) => {
      if (refs.every((ref) => !ref.current?.contains(e.target as Node))) {
        handler();
      }
    };
    document.addEventListener("click", handleClick);

    return () => {
      document.removeEventListener("click", handleClick);
    };
  }, [handler, refs]);
}
```

- usage
```tsx
import { TriangleDownIcon } from "@radix-ui/react-icons";
import BookmarksPopover from "./BookmarksPopover";
import { useRef, useState } from "react";
import { useOnClickOutside } from "../lib/hooks";

export default function BookmarksButton() {
  const [isOpen, setIsOpen] = useState(false);
  const buttonRef = useRef<HTMLButtonElement>(null);
  const popoverRef = useRef<HTMLDivElement>(null);
  useOnClickOutside([buttonRef, popoverRef], () => {
    setIsOpen(false);
  });

  return (
    <section>
      <button
        ref={buttonRef}
        className="bookmarks-btn"
        onClick={() => setIsOpen((prev) => !prev)}
      >
        Bookmarks <TriangleDownIcon />
      </button>
      {isOpen && <BookmarksPopover ref={popoverRef} />}
    </section>
  );
}
```

```tsx
import { forwardRef } from "react";
import { useBookmarksContext } from "../lib/hooks";
import JobList from "./JobList";

type BookmarksPopoverType = unknown;

const BookmarksPopover = forwardRef<HTMLDivElement, BookmarksPopoverType>(
  function (_, ref) {
    const { bookmarkedJobItems, isLoading } = useBookmarksContext();

    return (
      <div ref={ref} className="bookmarks-popover">
        <JobList jobItems={bookmarkedJobItems} isLoading={isLoading} />
      </div>
    );
  }
);

export default BookmarksPopover;
```
---
# Create Portal
- helps with dialog / modals to make sure they are always showing on top of content
```tsx
import { forwardRef } from "react";
import { useBookmarksContext } from "../lib/hooks";
import JobList from "./JobList";
import { createPortal } from "react-dom";

type BookmarksPopoverType = unknown;

const BookmarksPopover = forwardRef<HTMLDivElement, BookmarksPopoverType>(
  function (_, ref) {
    const { bookmarkedJobItems, isLoading } = useBookmarksContext();

    return createPortal(
      <div ref={ref} className="bookmarks-popover">
        <JobList jobItems={bookmarkedJobItems} isLoading={isLoading} />
      </div>,
      document.body
    );
  }
);
export default BookmarksPopover;
```


---

## Query Parameters

- Using `hashchange` event listener
```tsx
<a href={`#${jobItem.id}`} className="job-item__link">
```

```tsx
export function useActiveId() {
  const [activeId, setActiveId] = useState<number | null>(null);

  useEffect(() => {
    const handleHashChange = () => {
      const id = +window.location.hash.slice(1);
      setActiveId(id);
    };
    handleHashChange();

    window.addEventListener("hashchange", handleHashChange);

    return () => {
      window.removeEventListener("hashchange", handleHashChange);
    };
  }, []);

  return activeId;
}
```


---
## Fetching Data
- for fetching data when it is NOT in response to a user event, use `useEffect`
- for fetching data in response to a user event, fetch the data, in the event handler
	- react recommends data is fetched in the event handler whenever possible


## React Query (`@tanstack/react-query`)
(alternative is `swr` package by vercel)
```tsx
//main.tsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./components/App.tsx";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

const queryClient = new QueryClient();
ReactDOM.createRoot(document.getElementById("root")!).render(
	<QueryClientProvider client={queryClient}>
	  <App />
	</QueryClientProvider>
);
```

```tsx
export function useJobItem(id: number | null) {
  const { data, isLoading } = useQuery(
    ["job-item", id],
    async () => {
      const response = await fetch(`${BASE_API_URL}/${id}`);
      const data = await response.json();
      return data;
    },
    {
      staleTime: 1000 * 60 * 60,
      refetchOnWindowFocus: false,
      retry: false,
      enabled: Boolean(id),
      onError: () => {},
    }
  );

  return { jobItem: data?.jobItem as JobItemExpanded, isLoading };
}
```
- `react-query` will catch the error (no need for a `trycatch` block)


- get URL with `#` prefix
	- `<a href={#${id}}>Link</a>
	- `window.location.hash`


## React Query (Making Multiple Parallel Fetch)
- in google chrome, only 6 requests in parallel at once
```tsx
const fetchJobItem = async (id: number): Promise<JobItemApiResponse> => {
  const response = await fetch(`${BASE_API_URL}/${id}`);
  // 4xx or 5xx status code
  if (!response.ok) {
    const errorData = await response.json();
    throw new Error(errorData.description);
  }

  const data = await response.json();
  return data;
};
```

```tsx
export function useJobItems(ids: number[]) {
  const results = useQueries({
    queries: ids.map((id) => ({
      queryKey: ["job-item", id],
      queryFn: () => fetchJobItem(id),
      staleTime: 1000 * 60 * 60,
      refetchOnWindowFocus: false,
      retry: false,
      enabled: Boolean(id),
      onError: handleError,
    })),
  });

  const jobItems = results
    .map((result) => result.data?.jobItem)
    .filter((jobItem) => jobItem !== undefined);
  const isLoading = results.some((result) => result.isLoading);

  return { jobItems, isLoading };
}
```

---
## Prevent Bubbling Up Events
```tsx
import { BookmarkFilledIcon } from "@radix-ui/react-icons";
import { useContext } from "react";
import { BookmarksContext } from "../contexts/BookmarksContextProvider";

type BookmarkIconProps = { id: number };

export default function BookmarkIcon({ id }: BookmarkIconProps) {
  const { handleToggleBookmark } = useContext(BookmarksContext);

  return (
    <button
      onClick={(e) => {
        handleToggleBookmark(id);
        e.stopPropagation();
        e.preventDefault();
      }}
    >
      <BookmarkFilledIcon />
    </button>
  );
}
```


---
## `Zustand` Example
- using a `zustand` store will prevent unnecessary re-renders. It will only re-render for components that subscribe to the specific piece of state that has changed. (`useMemo` under the hood)
```jsx
import { create } from "zustand";
import { persist } from "zustand/middleware";
import { initialItems } from "../lib/constants";

type Item = {
  id: number;
  name: string;
  packed: boolean;
};

type ItemState = {
  items: Item[];
  addItem: (text: string) => void;
  deleteItem: (id: number) => void;
  getPackedItems: () => Item[];
};

export const useItemStore = create<ItemState>()(
  persist(
    (set, get) => ({
      items: initialItems,
      addItem: (newItemText: string) => {
        const newItem = { id: Date.now(), name: newItemText, packed: false };
        set((state) => ({ items: [...state.items, newItem] }));
      },
      deleteItem: (id: number) => {
        set((state) => {
          const newItems = state.items.filter((item) => item.id !== id);
          return { items: newItems };
        });
      },
      getPackedItems: () => {
        return get().items.filter((item) => item.packed);
      },
    }),
    // name of key in localStorage
    { name: "items" }
  )
);
```

---

## Children Composition
- prevents application from re-rendering
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

==example== filter out duplicates
- here `array.indexOf(company) === index` only returns true if the index of the current company is the same as the first time it is found, so if it is found later again it will return false (in other words will filter out the duplicates)
```ts
const companyList = feedbackItems
    .map((item) => item.company)
    .filter((company, index, array) => {
      return array.indexOf(company) === index;
    });
```

==example== count words in a string
```javascript
const numberOfWords = text.split(/\s/)
	.filter((word) => word !== "").length;
```