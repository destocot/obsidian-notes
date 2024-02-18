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

---

