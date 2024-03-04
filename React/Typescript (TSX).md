resources:
- https://github.com/total-typescript/ts-reset 

## Typing Props

```tsx
export default function Home() {
	return (
		<Button 
			backgroundColor="red" 
			fontSize={30}
			padding= {[5, 10, 20, 50]}
		/>
	)
}
```

```tsx
type Color = "red" | "blue" | "green";
type ButtonProps = {
	backgroundColor: Color;
	fontSize: number;
	pillShape?: boolean;
	padding: [number, number, number, number]
};

export default function Button({ 
	backgroundColor, 
	fontSize, 
	pillShape,
	padding 
}: ButtonProps) {
	return (<button>Click me</button>)
};
```

## Built-in Types

- style
```tsx
export default function Home() {
	return (
		<Button 
			style={{
				backgroundColor: "blue",
				fontSize: 24,
				color: "white"
			}}
		/>
	)
};
```

```tsx
type ButtonProps = { style: React.CSSProperties };

export default function Button({ style }: ButtonProps) {
	return (<button style={style}>Click me</button>)
};
```

- attributes of native elements
- without ref
```tsx
export default function Home() {
	return (<Button type="submit" autoFocus={true} />)
};
```

```tsx
type ButtonProps = React.ComponentPropsWithoutRef<"button">;

export default function Button({ type, autofocus }: ButtonProps) {
	return (<button type={type} autoFocus={autoFocus}>Click me</button>)
};
```

- with ref
```tsx
export default function Home() {
	return (<Button type="submit" autoFocus={true} ref={ref} />)
};
```

```tsx
type ButtonProps = React.ComponentPropsWithRef<"button">;

export default function Button({ type, autofocus, ref }: ButtonProps) {
	return (<button type={type} autoFocus={autoFocus}>Click me</button>)
};
```

## Rest
```tsx
export default function Home() {
	return (<Button type="submit" autoFocus={true} clasName="btn" />)
};
```

```tsx
type ButtonProps = React.ComponentPropsWithoutRef<"button">;

export default function Button({ type, ...rest }: ButtonProps) {
	return (<button type={type} {...rest}>Click me</button>)
};
```

## Intersect Types
```tsx
export default function Home() {
	return (<Button type="submit" autoFocus={true} clasName="btn" />)
};
```

```tsx
type ButtonProps = React.ComponentPropsWithoutRef<"button"> & {
	variant?: "primary" | "secondary";
}

export default function Button({ type, variant, ...rest }: ButtonProps) {
	return (<button type={type} {...rest}>Click me</button>)
};
```

## Record Type
```tsx
export default function Home() {
	return (
		<Button 
			borderRadius={{
				topLeft: 5,
				topRight: 5,
				bottomRight: 10,
				bottomLeft: 10
			}}
		/>
	)
};
```

```tsx
type ButtonProps = {
	borderRadius: {
		Record<string, number>
	}
};

export default function Button({ borderRadius }: ButtonProps) {
	return (<button>Click me</button>)
};
```

## Function
```tsx
export default function Home() {
	const clickHandler = (weight: number) => { return weight > 150 };

	return (<Button onClick={clickHandler} />)
};
```

```tsx
type ButtonProps = { onClick: (w: number) => boolean };

export default function Button({ onClick }: ButtonProps) {
	return (<button onClick={onClick}>Click me</button>)
};
```

## Children
```tsx
export default function Home() {
	return (<Button>Click me</Button>)
};
```

- `children: React.ReactNode` is less strict than `children: JSX.Element`
```tsx
type ButtonProps = { children: React.ReactNode };

export default function Button({ children }: ButtonProps) {
	return (<button>{ children }</button>)
};
```

## State
```tsx
export default function Home() {
	const [count, setCount] = useState(0);
	
	return (<Button setCount={setCount} />)
};
```

```tsx
type ButtonProps = { setCount: React.Dispatch<React.SetStateAction<number>> };

export default function Button({ setCount }: ButtonProps) {
	return (<button>Click me!</button>)
};
```

## Default Values

```tsx
type ButtonProps = { setCount: React.Dispatch<React.SetStateAction<number>> };

export default function Button({ count = 0 }: ButtonProps) {
	return (<button>Click me!</button>)
};
```
## Caveats With Interface
```ts
interface ButtonProps {
	text: string;
	count: number;
}
```
- interface can only be used to describe objects
- to combine interfaces we use the `extends` keyword (versus the intersection `&`)
- interfaces have better error messages
- interfaces can be merged
	- "interfaces are open" and "type aliases are closed"

## `onClick`
```tsx
const handleClick = (evt: React.MouseEvent<HTMLButtonElement, MouseEvent>) => {
	console.log("clicked");
}

return <button onClick={handleClick}>Click me!</button>
```

## Hooks
- `useState`
- most of the time typescript can infer the type
```tsx
const [count, setCount] = useState<number>(0);
```

```tsx
type User = { name: string; age: number; }
const [user, setUser] = useState<User | null>(null);
```

- `useRef`
```tsx
const ref = useRef<HTMLButtonElement>(null);

return <button ref={ref}>Click me!</button>
```

## `as const`
- this makes the type read only, so that you can view the actual values
```tsx
const textOptions = [ "hi", "hey", "hello" ] as const;

return <button>
		{textOptions.map((option) => { 
			return option 
		})}
	</button>
```

## Omit

```tsx
type User = {
	sessionId: string;
	name: string
};

type Guest = Omit<User, "name">;
```

## Assert Types
```tsx
type ButtonColor = "red" | "blue" | "green";

useEffect(() => {
	const prevButtonColor = localStorage.getItem("buttonColor") as ButtonColor;
}, []);
```

## Generics
- specifies a relationship (more general)
- for `JSX` need to add a comma for to differentiate it from a `JSX` element
```tsx
const convertToArray = <T,>(value: T): T[] => { return [value] };

convertToArray(5);
convertToArray("Hello");
```
- avoid comma by using a function expression
```tsx
function convertToArray<T>(value: T): T[] { return [value] };
```
- restrict type
```tsx
function convertToArray<T extends number | string>(value: T): T[] { 
	return [value] 
};
```


- specifying a relationship between multiple props
```tsx
return <>
	<Button countValue={5} countHistory={[10, 20, 30]} />
	<Button countValue={"5"} countHistory={["10", "20", "30"]} />
	</>
```

```tsx
type ButtonProps<T> = {
	countValue: T;
	countHistory: T[];
}

export default function Button<T>({
	countValue, countHistory
}: ButtonProps<T>) {
	return <button>Click me!</button>;
}
```

- another example with props
```tsx
export default function Themes() {
	const [selectedTheme, setSelectedTheme] = useState("light");
	const themeOptions = ["light", "dark", "system"];

	return (
		<section>
			<ThemeOptions 
				themeOptions={themeOptions} 
				selectedTheme={selectedTheme}
				onThemeClick={(theme) => setSelectedTheme(theme)}
			/>
		</section>
	)
}

type ThemeOptionsProps<T> = {
	themeOptions: T[];
	selectedTheme: T;
	onThemeClick: (theme: T) => void;
}

function ThemeOptions<T extends React.ReactNode>({
	themeOptions,
	selectedTheme,
	onThemeClick
}: ThemeOptionsProps<T>) {
	return (
		<ul>
			{themeOptions.map((theme, index) => (
				<li>
					<button onClick={() => onThemeClick(theme)}}>
						{theme}
					</button>
				</li>
			))}
		</ul>
	)
}
```

- relationship with multiple types
```ts
function createArrayPair<T, K>(input1: T, input2: K): [T, K] {
	return [input1, input2];
}

createArrayPair("hello", 10);
```

## Reuse Types

```tsx
// types.ts
export type Color = "red" | "blue" | "color"
```

```tsx
import { type Color } from "@/lib/types"
// or import type { Color } from "@/lib/types"
```

## Extracting Types
```ts
const project = {
	title: "Project 1",
	specification: {
		areaSize: 100,
		rooms: 3
	}
}

type Specification = typeof project["specification"]
```

## Unknown Type
- unknown encourages the verify the data we get back
```tsx
useEffect(() => {
	 fetch("https://jsonplaceholder.typicode.com/todos/1")
		 .then((response) => response.json())
		 .then((date: unknown) => {
			 // run through a validation library
			 // ...
		 })
}, [])
```

## Classes
```ts
type TUser {
	name: string;
	age: number;
}

class User implements TUser {
	name: string;
	age: number;

	constructor(name: string, age: number) {
		this.name = name;
		this.age = age;
	}
}
```

