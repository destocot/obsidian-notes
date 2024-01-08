1. create next.js project 
```
npx create-next-app@latest
```
2. clean up files
3. upload to GitHub repository
4. install shadcn/ui
```
npx shadcn-ui@latest init
```
5. create sign-in page
6. install form component from shadcn/ui
```
npx shadcn-ui@latest add form
```
7. install input component from shadcn/ui
```
npx shadcn-ui@latest add input
```
8. create sign-in form
9. create sign-up page / sign-up form
10. add links between sign-in and sign-up

> end of part 1

11. install shadcn/ui theme
12. return home button
13. install prisma CLI
```
npx prisma
```
14. initialize prisma
```
npx prisma init
```
15. get postgres database string
```
postgresql://<username>:<password>@localhost:5432/mydb?schema=public
```
16. create user, account, and profile models
```
https://authjs.dev/reference/adapter/prisma
```
17. migrate prisma
```
npx prisma migrate dev --name <name>
```
18. install prisma client
```
npm install @prisma/client
```
19. [create prisma singleton](https://www.prisma.io/docs/orm/more/help-and-troubleshooting/help-articles/nextjs-prisma-client-dev-practices#solution)
20. TEST | prisma query
```
db.user.findMany()
```

> end of part 2

21. create actions folder
22. create sign-up action
23. set-up error state
24. set-up router
25. hash password with bcryptjs
```
npm install bcryptjs @types/bcryptjs
```
26. route to sign-in page
27. install shadcn/ui sonner
```
npx shadcn-ui@latest add sonner
```
28. add toaster
29. create toast on sign-up page
30. confirm data in database

>end of part 3

31. install next-auth
```
npm install next-auth@beta
```
32. create `auth.ts`
33. create `/api/auth/[...nextauth]`
34. configure Credentials Provider
35. create `AUTH_SECRET, AUTH_URL` environment variables
```
openssl rand -hex 32
```
36. configure basic sign-in flow
37. TEST | sign out
38. create secret page for practice
39. setup `middleware.ts`
40. fix sign-in route

> end of part 4

41. auth layout redirects
42. attach id to session
43. typescript session
44. add accounts table
45. install prisma-adapter
```
npm install @auth/prisma-adapter
```
46. prepare for google OAuth
47. create `AUTH_GOOGLE_CLIENT_ID, AUTH_GOOGLE_CLIENT_SECRET` environment variables
48. google console cloud
```
https://console.cloud.google.com/getting-started
```
49. add to providers array
50. create google sign-in button

> end of part 5

51. set-up usernames for google OAuth users
52. create profile page
53. create main layout
54. move home page to (main)
55. lightly style main layout / home page / profile page
56. prevent email collisions
57. implement GitHub OAuth
58. taking a look at Discogs API
59. create Album model
60. query profile albums

> end of part 6

61. tailwind prettier plugin
```
npm install -D prettier prettier-plugin-tailwindcss
```

```
{ "plugins": ["prettier-plugin-tailwindcss"] }
```

62. create a search albums page
63. next.js image hosting
64. setup search params
65. TEST | searching
66. install shadcn/ui card
67. create cards
68. create overlay card with button
69. create add button
70. TEST | add functionality

> end of part 7

71. check profile page
72. fix mobile responsibility
73. profile page layout
74. main album card
75. home page
75. individual album page
76. album details skeleton
77. error pages
78. profile display
79. dark mode
80. mobile recheck

> end of part 8

81. active links
82. remove albums
83. infinite scroll
84. filter albums
85. ...