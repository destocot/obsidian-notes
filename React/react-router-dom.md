 ## installation
```bash
npm install react-router-dom
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


## Router Parameters
```tsx
const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path="/" element={<RootLayout />}>
      <Route index element={<Home />} />
      <Route path="about" element={<About />} />
      <Route path="careers" element={<CareersLayout />}>
        <Route index element={<Careers />} loader={careersLoader} />
        <Route 
	        path=":id" 
	        element={<CareerDetails />} 
	        loader={careerDetailsLoader} 
        />
      </Route>
    </Route>
  )
);
```

```tsx
const PathNames = {
  careerDetails: "/careers/:id",
} as const;

interface Args extends ActionFunctionArgs {
  params: Params<ParamParseKey<typeof PathNames.careerDetails>>;
}

export async function careerDetailsLoader({ params }: Args) {
  const { id } = params;
  const response = await fetch(`http://localhost:4000/careers/${id}`);
  return response.json();
}
```

```tsx
import { useLoaderData } from "react-router-dom";
import { CareerType } from "../../types";

export default function CareerDetails() {
  const career = useLoaderData() as CareerType;

  return (<pre>{JSON.stringify(career)}</pre>);
}
```


## Handling Errors in Dynamic Routes
```tsx
const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path="/" element={<RootLayout />}>
      <Route index element={<Home />} />
      <Route path="about" element={<About />} />
      <Route path="careers" element={<CareersLayout />}>
        <Route index element={<Careers />} loader={careersLoader} />
        <Route
          path=":id"
          element={<CareerDetails />}
          loader={careerDetailsLoader}
          errorElement={<CareerDetailsError />}
        />
      </Route>
    </Route>
  )
);
```

```tsx
import { Link, useRouteError } from "react-router-dom";

export default function CareerDetailsError() {
  const error = useRouteError();

  return (
    <div className="careers-error">
      <h2>Error</h2>
      <p>
	      {error instanceof Error ? error.message : "Something went wrong."}
      </p>
      <Link to="/">Back to homepage</Link>
    </div>
  );
}
```

```tsx
const PathNames = { careerDetails: "/careers/:id" } as const;

interface Args extends ActionFunctionArgs {
  params: Params<ParamParseKey<typeof PathNames.careerDetails>>;
}

export async function careerDetailsLoader({ params }: Args) {
  const { id } = params;
  
  const response = await fetch(`http://localhost:4000/careers/${id}`);
  if (!response.ok) throw Error("Could not find details for this career.");
  return response.json();
}
```


## Share Error Element

- adding the ```errorElement```  to the layout will provide a `errorElement` for all children
- caveat: the layout is not rendered for any routes that render this `errorElement` (whereas if a child element has its own `errorElement` then the layout would be rendered)

```tsx
import { Link, useRouteError } from "react-router-dom";

export default function CareerDetailsError() {
  const error = useRouteError();

  return (
    <div className="careers-error">
      <h2>Error</h2>
      <p>
	      {error instanceof Error ? error.message : "Something went wrong."}
      </p>
      <Link to="/">Back to homepage</Link>
    </div>
  );
}
```

```tsx
const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path="/" element={<RootLayout />}>
      <Route index element={<Home />} />
      <Route path="about" element={<About />} />
      <Route
        path="careers"
        element={<CareersLayout />}
        errorElement={<CareersError />}
      >
        <Route index element={<Careers />} loader={careersLoader} />
        <Route
          path=":id"
          element={<CareerDetails />}
          loader={careerDetailsLoader}
        />
      </Route>
      <Route path="*" element={<NotFound />} />
    </Route>
  )
```

```tsx
export async function careersLoader() {
  const response = await fetch("http://localhost:4000/careers");
  if (!response.ok) throw Error("Could not retrieve careers list.");
  return response.json();
}

const PathNames = { careerDetails: "/careers/:id" } as const;

interface Args extends ActionFunctionArgs {
  params: Params<ParamParseKey<typeof PathNames.careerDetails>>;
}

export async function careerDetailsLoader({ params }: Args) {
  const { id } = params;
  
  const response = await fetch(`http://localhost:4000/careers/${id}`);
  if (!response.ok) throw Error("Could not find details for this career.");
  return response.json();
}

```

## `useLocation` hook
```tsx
import { useLocation } from "react-router-dom";
const location = useLocation();
/* 
{
	pathname: '/help/contact',
	search: '',
	hash: '',
	state: null,
	key: 'b0ahhp5t'
} 
*/
```

## Breadcrumbs

```tsx
import { Link, useLocation } from "react-router-dom";

export default function Breadcrumbs() {
  const location = useLocation();
  let currentLink = "";

  const crumbs = location.pathname.split("/")
    .filter((crumb) => crumb !== "")
    .map((crumb) => {
      currentLink += `/${crumb}`;
      return (
        <div className="crumb" key={crumb}>
          <Link to={currentLink}>{crumb}</Link>
        </div>
      );
    });

  return <div className="breadcrumbs">{crumbs}</div>;
}
```

## Forms

```tsx
import { Form } from "react-router-dom";

export default function Contact() {
  return (
    <div>
      <h3>Contact Us</h3>
      <Form method="POST" action="/help/contact">
        <label>
          <span>Your email:</span>
          <input type="email" name="email" required />
        </label>
        <label>
          <span>Your message:</span>
          <textarea name="message" required></textarea>
        </label>
        <button>Submit</button>
      </Form>
    </div>
  );
}
```

## Actions

```tsx
const PathNames = { careerDetails: "/careers/:id" } as const;

interface Args extends ActionFunctionArgs {
  params: Params<ParamParseKey<typeof PathNames.careerDetails>>;
}

export const contactAction = async ({ request }: Args) => {
  const data = await request.formData();

  const submission = { email: data.get("email"), message: data.get("message") };
  console.log(submission);

  return null;
};
```

```tsx
const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path="/" element={<RootLayout />}>
      <Route index element={<Home />} />
      <Route path="about" element={<About />} />
      <Route path="help" element={<HelpLayout />}>
        <Route path="faq" element={<Faq />} />
        <Route path="contact" element={<Contact />} action={contactAction} />
      </Route>
    </Route>
  )
);
```

## Handle Errors and Redirects in Actions
```tsx
import { redirect } from "react-router-dom";

const PathNames = { careerDetails: "/careers/:id" } as const;

interface Args extends ActionFunctionArgs {
  params: Params<ParamParseKey<typeof PathNames.careerDetails>>;
}

export const contactAction = async ({ request }: Args) => {
  const data = await request.formData();

  const submission = {
    email: data.get("email") as string,
    message: data.get("message") as string,
  };
  console.log(submission);

  // send post request
  if (submission.message.length < 10) {
    return { error: "Message must be 10+ characters." };
  }

  return redirect("/");
};

```

```tsx
import { Form, useActionData } from "react-router-dom";
import { ContactResponseType } from "../../types";

type ContactResponseType = { error: string } | undefined;

export default function Contact() {
  const data = useActionData() as ContactResponseType;

  return (
    <div className="contact">
      <h3>Contact Us</h3>
      <Form method="POST" action="/help/contact">
        <label>
          <span>Your email:</span>
          <input type="email" name="email" required />
        </label>
        <label>
          <span>Your message:</span>
          <textarea name="message" required></textarea>
          {data?.error && <p>{data.error}</p>}
        </label>
        <button>Submit</button>
      </Form>
    </div>
  );
}
```

## Navigate Component
- mocking user session
- `replace={true}` replaces the path in the history
```tsx
import { useState } from "react";
import { Navigate } from "react-router-dom";

type MockUserType = { username: string };

export default function About() {
  const [user, setUser] = useState<MockUserType | null>({ username: "mario" });
  if (!user) return <Navigate to="/" replace={true} />

  return (
    <div className="about">
      <h2>About Us</h2>
      <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit.</p>
      <button onClick={() => setUser(null)}>Logout</button>
    </div>
  );
}
```











---
## older way of making basic routes (`v5?`)
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