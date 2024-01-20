
1. create next.js app

```
npx create-next-app@latest
```

2. remove boiler plate code

3. install next-auth

```
npm install next-auth@beta
```

4. environment variables
	1. NEXTAUTH_URL = http://localhost:3000
	2. NEXTAUTH_SECRET = 
	
```
openssl rand -hex 32
```

5. create `auth.config.ts`

6. create `middleware.ts`

7. create `auth.ts`

8. create types in `/types/next-auth.d.ts` as necessary

9.  create sign-in page

10.  create sign-in action

> end of part 1

11. add `useFormState` and `useFormStatus`

12. TEST | sign-in flow

13. create sign-out button

14. initialize Prisma `npx prisma init`

15. move to single .env (add to .gitignore)

16. connect to local postgres database

17. create Profile model in `schema.prisma`

```
model Profile {
  id        String   @id @default(uuid())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  email    String  @unique
  display  String  @unique
  password String
  image    String?
  isAdmin  Boolean @default(false)
}
```

18. map data model to the database schema `npx prisma migrate dev init`

19. install Prisma client

```
npm install @prisma/client
```

20. create Prisma singleton 

> end of part 2

21. TEST | `prisma.profile.findMany()`

22. create signup page

23. create api route `/api/auth/signup/route.ts`

24. validate body

25. confirm passwords

26. check if email or username is taken
	1. try to use OR statement

28. hash password

```
npm install bcryptjs
```

30. create profile `prisma.profile.create({})`

31. check sign-in flow

> end of part 3

31. create `getProfileFromSessionId` helper function

32. lightly style forms

33. tailwind forms plugin

34. tailwind typography plugin

35. `/api/auth/[...nextauth]`

36.  GitHub env files and callback

```
http://localhost:3000/api/auth/callback/github
```

```
 process.env.GITHUB_ID
 process.env.GITHUB_SECRET
```

37. GitHub sign-in flow

38. Google env files and callback

39. Google sign-in flow

40. lightly style forms `@nextui-org/react`

> end of part 4

41. react icons

```
npm install react-icons
```

42.  create main layout

43. navbar

 44. active links

45. footer

46. 