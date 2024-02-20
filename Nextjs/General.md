## Suspense
```
v src
	v app
		v posts
			> [id]
			loading.tsx
			page.tsx
```

- the `loading.tsx` file is a special file that lets Next.js know what you want as a fallback when something is loading.
```tsx
// loading.tsx
export default function Loading() {
	return <div className="text-center mt-14">Loading...</div>
}
```

```tsx
// page.tsx
import PostList from "@/components/posts-list";
import prisma from "@/lib/db";

export default async function PostsPage() {
	const posts = await prisma.post.findMany();

	return (
		<main className="text-center pt-16 px-5">
			<h1 className="text-5xl font-semibold mb-7">All posts</h1>
			<PostsList posts={posts} />
		</main>
	)
}
```
- behind the scenes Next.js will take the `loading.tsx` component and wrap the page at the same file level with a `<Suspense fallback={<Loading />}></Suspense>`

**Keeping Suspense With Query Parameters**
- give the `Suspense` component a unique key to let the component know when it is a new route
```tsx
 <Suspense fallback={<Loading />} key={city + page}>
        <EventsListFetcher city={city} page={+page} />
</Suspense>
```

## Streaming
- Suspense, with the `loading.tsx` file will by default prevent the whole page from loading until the data is fetched **(this is not the best user experience)**

 - one solution would be to move the data fetching directly into the component that uses the data
 - we can manually wrap a component in suspense
 - when the render is complete it will be streamed in
 ```tsx
 // page.tsx
import PostList from "@/components/posts-list";
// import prisma from "@/lib/db";
export default async function PostsPage() {
	// const posts = await prisma.post.findMany();

	return (
		<main className="text-center pt-16 px-5">
			<h1 className="text-5xl font-semibold mb-7">All posts</h1>
			<Suspense fallback={<div>Loading...</div>}>
				<PostsList /*posts={posts}*/ />
			</Suspense>
		</main>
	)
}
```

```tsx
// posts-list.tsx
import Link from "next/link";
import prisma from "@/lib/db";

export default async function PostsList() {
	const posts = await prisma.post.findMany();

	return (
		<>
			<ul>
				{posts.map((post) => (
					<li key={post.id} className="max-w-[400px] mb-3 mx-auto">
						<Link href={`/posts/${post.id}`}>{post.title}</Link>
					</li>
				))}
			</ul>
		</>
	)
}
```

## Caching
- on production build dynamic routes will be pre-rendered as static pages, we can force dynamic behavior with the following export
```tsx
export const dynamic = "force-dynamic";

export default async function PostsPage() {
	const random = Math.floor(Math.random() * 10) + 1;
	const res = await fetch(`https://dummyjson.com/posts?limit=${random}`);

	/* ... */
}
```
- this will make the component and all child components dynamic
- if we want to be more granular we can do the following
```tsx
export default async function PostsPage() {
	const random = Math.floor(Math.random() * 10) + 1;
	const res = await fetch(`https://dummyjson.com/posts?limit=${random}`, {
		cache: "no-cache",
	});
	/* ... */
}
```

- we will cache the page, but after 3600 seconds (1 hour) we will get a new fetch call
```tsx
export default async function PostsPage() {
	const random = Math.floor(Math.random() * 10) + 1;
	const res = await fetch(`https://dummyjson.com/posts?limit=${random}`, {
		next: {
			revalidate: 3600
		}
	});
	/* ... */
}
```

- Server Side Caching
- Next.js extends the native fetch API to include options to control caching
```tsx
// will send network request on every reload
const response = await fetch(
    `https://bytegrad.com/course-assets/projects/evento/api/events?city=${city}`,
    {
      cache: "no-cache", 
    },
  );
```

```tsx
// will revlidate data every 300 seconds (5 minutes)
const response = await fetch(
    `https://bytegrad.com/course-assets/projects/evento/api/events?city=${city}`,
    {
		next: {
			revalidate: 300
		}
    },
  );
```

---

## Rendering Server Component inside a Client Component
- utilizing the children pattern
```tsx
import ExampleClientComponent from "@/components/example-client-component";
import ExampleServerComponent from "@/components/example-server-component";

export default function Home() {
	return (
		<main>
			<p>1. This text is in  a server component</p>
			<ExampleClientComponent>
				<ExampleServerComponent />
			</ExampleClientComponent>
		</main>
	)
}
```

> usually, this is important when utilizing contexts (which are client components). The misconception is that everything inside the context will be a client component, which is not true if we utilize the children pattern.

## `not-found.tsx`

```tsx
export default function NotFound() {
  return <main>We could not find that page</main>;
}
```
- can be nested for more specific not found rendering

**TO EXPLORE:**
- advanced pattern: data fetching wrapper component
```tsx
 <Suspense fallback={<Loading />}>
	 <WrapperComponent>
		<EventsList city={city} />
	 </WrapperComponent>
  </Suspense>
```
- this will allow the events list to be more more reusable because the wrapper component will be responsible for fetching data instead of the component itself

## `error.tsx`
- will create an error boundary wrapped around your application
- example from next.js documentation
```tsx
"use client"; // Error components must be Client Components

import H1 from "@/components/h1";
import { useEffect } from "react";

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  useEffect(() => {
    // Log the error to an error reporting service
    console.error(error);
  }, [error]);

  return (
    <main className="py-24 text-center">
      <H1 className="mb-5">Something went wrong!</H1>
      {/* <H1>{error.message}</H1> */}
      <button
        onClick={
          // Attempt to recover by trying to re-render the segment
          () => reset()
        }
      >
        Try again
      </button>
    </main>
  );
}
```

---
## Metadata
```tsx
import type { Metadata } from "next";

export const metadata: Metadata = {
	title: "Evento - Find events around you",
	description: "Browse more than 10,000 events worldwide",
};
```

- dynamic metadata
```tsx
import type { Metadata } from "next";
import { capitalize } from "@/lib/utils";
type Props = { params: { city: string; }; };

export function generateMetadata({ params }: Props): Metadata {
  const city = params.city;

  return {
    title: city === "all" ? 
    "All Events" : 
    `Events in ${capitalize(city)}`,
  };
}

export default async function EventsPage({ params }: Props) {
	return <div>{/* ... */}</div>;
```

## Middleware
- common use cases redirects / authentication

- simple example for redirects
```tsx
import { NextResponse } from "next/server";

export function middleware(request: Request) {
  return NextResponse.redirect(new URL("/events/all", request.url));
}

export const config = {
  matcher: ["/events"],
};
```



----
-----
# Tailwind

## Extend Theme
```typescript
// tailwind.config.ts
import type { Config } from "tailwindcss";
const config: Config = {
  content: [ /* ... */ ],
  theme: {
    extend: {
      colors: {
        accent: "#a4f839",
      },
    },
  },
  plugins: [],
};
export default config;
```

## `cn` utility function
```tsx
//utils.ts
import { ClassValue, clsx } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

## Use Image As Background
```tsx
<main>
      <section className="relative flex py-14 md:py-20 items-center justify-center overflow-hidden">
        <Image
          src={event.imageUrl}
          className="z-0 object-cover blur-3xl"
          alt="Event background image"
          fill
          quality={50}
          sizes="(max-width: 1280px) 100vw, 1280px"
          priority
        />
        <div className="z-1 relative flex flex-col">
          <Image
            src={event.imageUrl}
            alt={event.name}
            width={300}
            height={201}
          />
          <div>
            <H1>{event.name}</H1>
            <p>Organized by {event.organizerName}</p>
            <button>Get tickets</button>
          </div>
        </div>
      </section>
      <div></div>
    </main>
```

## Simple Skeleton
```tsx
import { cn } from "@/lib/utils";

type SkeletonProps = React.ComponentPropsWithoutRef<"div">;

export default function Skeleton({ className }: SkeletonProps) {
  return (
    <div
      className={cn(
        "h-4 w-[550px] animate-pulse rounded-md bg-white/5",
        className,
      )}
    />
  );
}
```


---
---
# Framer Motion

- underline navigation link animation
```tsx
{pathname === route.href && (
	<motion.div
	  layoutId="header-active-link"
	  className="absolute bottom-0 h-1 w-full bg-accent"
	/>
)}
```

- fade in card animation
```tsx
"use client";

import { motion, useScroll, useTransform } from "framer-motion";
import Link from "next/link";
import { useRef } from "react";

type MotionLinkProps = {
  children: React.ReactNode;
  slug: string;
};

const MLink = motion(Link);

export default function MotionLink({ children, slug }: MotionLinkProps) {
  const ref = useRef(null);
  const { scrollYProgress } = useScroll({
    target: ref,
    offset: ["0 1", "1.5 1"],
  });
  const scaleProgress = useTransform(scrollYProgress, [0, 1], [0.8, 1]);
  const opacityProgress = useTransform(scrollYProgress, [0, 1], [0.3, 1]);

  return (
    <MLink
      ref={ref}
      href={`/event/${slug}`}
      className="h-[380px] max-w-[500px] flex-1 basis-80"
      style={{
        // @ts-ignore
        scale: scaleProgress,
        // @ts-ignore
        opacity: opacityProgress,
      }}
      initial={{ opacity: 0, scale: 0.8, }}
    >
      {children}
    </MLink>
  );
}
```

```tsx
import { EventoEvent } from "@/lib/types";
import MotionLink from "./motion-link";

type EventCardProps = {  event: EventoEvent };

export default function EventCard({ event }: EventCardProps) {
  return (
    <MotionLink slug={event.slug}>
	    { /* ... */ }
    </MotionLink>
  );
}
```