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


