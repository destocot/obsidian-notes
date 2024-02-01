## installation
```bash
npm install react-router-domnpm install react-router-dom
```

## basic routes (v6)
```tsx
import { 
	createBrowserRouter, Route, createRoutesFromElements, RouterProvider
} from "react-router-dom";
import { About, Home } from "./pages";

const router = createBrowserRouter(
  createRoutesFromElements(
    <Route>
      <Route index element={<Home />} />
      <Route path="about" element={<About />} />
    </Route>
  )
);

export default function App() {
  return (
    <RouterProvider router={router} />
  );
}
```

## index route
- these two route components are equivalent
```tsx
<Route path="/" element={<Home />} />
<Route index element={<Home />} />
```

## Link / `NavLink`
- when the `pathname` is equal to the to attribute of a `NavLink` component, `react-router-dom` adds a "active" class onto it, which can be used for styling.
```tsx
<nav>
	<Link to="/">Home</Link>
	<NavLink to="/about">About</NavLink>
</nav>
```

```css
.active { /* styles */ }
```

## Layouts
	- `<Outlet />` renders the children elements
```tsx
const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path="/" element={<RootLayout />}>
      <Route index element={<Home />} />
      <Route path="about" element={<About />} />
    </Route>
  )
);
```

```tsx
import { NavLink, Outlet } from "react-router-dom";

export default function RootLayout() {
  return (
    <div>
        <nav>
          <NavLink to="/">Home</NavLink>
          <NavLink to="/about">About</NavLink>
        </nav>
        <Outlet />
    </div>
  );
}
```

## Nested Routes
- Here to access the `<Faq />`  component, one would navigate to `/help/faq`
- The `<Faq />` component renders the layout of the `<HelpLayout />` which also renders the `<RootLayout />` since it is nested inside of it 
```tsx
const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path="/" element={<RootLayout />}>
      <Route index element={<Home />} />
      <Route path="about" element={<About />} />
      <Route path="help" element={<HelpLayout />}>
        <Route path="faq" element={<Faq />} />
      </Route>
    </Route>
  )
);
```

## `errorElement` (404 Page)
- Using the `path="*"` creates a catch all route where can create a custom error page
- the catch all route also applies any layouts it is nested in.
```tsx
const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path="/" element={<RootLayout />}>
      <Route index element={<Home />} />
      <Route path="about" element={<About />} />
      <Route path="*" element={<NotFound />} />
    </Route>
  )
);
```

## Loaders

> requires: `createBrowserRouter`

- Allow data to be loaded into a component before it renders.

- create a loader function
```tsx
export default async function careersLoader() {
  const response = await fetch("http://localhost:4000/careers");
  return response.json();
}
```


- attach loader to the router
```tsx
const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path="/" element={<RootLayout />}>
      <Route index element={<Home />} />
      <Route path="about" element={<About />} />
      <Route path="careers" element={<CareersLayout />}>
        <Route index element={<Careers />} loader={careersLoader} />
      </Route>
    </Route>
  )
);
```

- use `useLoaderData()` hook from `react-router-dom` to retrieve the data
```tsx
import { useLoaderData } from "react-router-dom";
type CareerType = {  id: string;  title: string; };

export default function Careers() {
  const careers = useLoaderData() as CareerType[];
  return (
    <div>
      {careers.map((career) => (
          <p key={career.id}>{career.title}</p>
      ))}
    </div>
  );
}
```






---
## older way of making basic routes
```tsx
import { BrowserRouter, Routes, Route} from "react-router-dom";
import { About, Home } from "./pages";

function App() {
  return (
    <BrowserRouter>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="about" element={<About />} />
        </Routes>
    </BrowserRouter>
  );
}
export default App;
```