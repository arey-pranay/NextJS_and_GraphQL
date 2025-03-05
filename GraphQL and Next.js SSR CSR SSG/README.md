# NextJS_and_GraphQL

---

## 🚀 **GraphQL in Next.js: Client & Server Fetching**
To fetch GraphQL data in **Next.js**, you can use:

1. **Client-Side Fetching**: Using Apollo Client or Fetch API.
2. **Server-Side Fetching**:
   - **Server-side Rendering (SSR)**
   - **Static Site Generation (SSG)**
   - **API Routes (Middleware for GraphQL proxying)**

---

## 🛠 **1. Install Required Dependencies**
You’ll need **Apollo Client** (popular GraphQL client) for client-side and `graphql-request` (lightweight GraphQL fetcher) for server-side fetching.

### Install:
```sh
npm install @apollo/client graphql
npm install graphql-request
```

---

## 📌 **2. Setting Up Apollo Client for Client-Side Fetching**
Apollo Client is used for **client-side GraphQL queries**.

### 🔹 **Create an Apollo Client Instance**
Create a file `lib/apollo-client.ts`:
```ts
import { ApolloClient, InMemoryCache } from "@apollo/client";

const client = new ApolloClient({
  uri: "https://your-graphql-api.com/graphql", // Replace with actual API
  cache: new InMemoryCache(),
});

export default client;
```

---

## 📌 **3. Using Apollo Client in a Client Component**
If you have a React **Client Component**, use the `useQuery` hook from Apollo.

### 🔹 **Example: Fetching Data in a Client Component**
```tsx
"use client"; // Ensures this runs on the client-side

import { gql, useQuery } from "@apollo/client";
import client from "@/lib/apollo-client";

const GET_USERS = gql`
  query {
    users {
      id
      name
      email
    }
  }
`;

export default function Users() {
  const { data, loading, error } = useQuery(GET_USERS);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return (
    <div>
      <h2>Users List</h2>
      <ul>
        {data.users.map((user: any) => (
          <li key={user.id}>{user.name} - {user.email}</li>
        ))}
      </ul>
    </div>
  );
}
```

**🔹 Notes:**
- The `useQuery` hook handles fetching and caching.
- `loading` and `error` states are handled automatically.

---

## 📌 **4. Fetching GraphQL Data in a Server Component (SSR & SSG)**
For **server-side fetching**, use `graphql-request` instead of Apollo Client.

### 🔹 **Example: Fetching GraphQL Data in a Server Component**
```tsx
import { request, gql } from "graphql-request";

const GET_USERS = gql`
  query {
    users {
      id
      name
      email
    }
  }
`;

export default async function UsersPage() {
  const data = await request("https://your-graphql-api.com/graphql", GET_USERS);

  return (
    <div>
      <h2>Users (Server-Side)</h2>
      <ul>
        {data.users.map((user: any) => (
          <li key={user.id}>{user.name} - {user.email}</li>
        ))}
      </ul>
    </div>
  );
}
```

**🔹 Why Use `graphql-request`?**
- Works great in **server components**.
- Lightweight compared to Apollo.
- No need for a client-side GraphQL cache.

---

## 📌 **5. Fetching GraphQL Data with SSR (getServerSideProps)**
If you want **SSR**, use `getServerSideProps` inside a `page.tsx` file.

### 🔹 **Example: SSR with `getServerSideProps`**
```tsx
import { request, gql } from "graphql-request";

const GET_USERS = gql`
  query {
    users {
      id
      name
      email
    }
  }
`;

export async function getServerSideProps() {
  const data = await request("https://your-graphql-api.com/graphql", GET_USERS);

  return { props: { users: data.users } };
}

export default function Users({ users }: { users: any[] }) {
  return (
    <div>
      <h2>Users (SSR)</h2>
      <ul>
        {users.map((user) => (
          <li key={user.id}>{user.name} - {user.email}</li>
        ))}
      </ul>
    </div>
  );
}
```

**🔹 Why SSR?**
- **Data is always fresh** (useful for dynamic content).
- Slightly slower than SSG due to on-request fetching.

---

## 📌 **6. Fetching GraphQL Data with SSG (getStaticProps)**
If the data doesn’t change often, use **SSG** for better performance.

### 🔹 **Example: SSG with `getStaticProps`**
```tsx
import { request, gql } from "graphql-request";

const GET_USERS = gql`
  query {
    users {
      id
      name
      email
    }
  }
`;

export async function getStaticProps() {
  const data = await request("https://your-graphql-api.com/graphql", GET_USERS);

  return { props: { users: data.users }, revalidate: 10 }; // Rebuild every 10s
}

export default function Users({ users }: { users: any[] }) {
  return (
    <div>
      <h2>Users (SSG)</h2>
      <ul>
        {users.map((user) => (
          <li key={user.id}>{user.name} - {user.email}</li>
        ))}
      </ul>
    </div>
  );
}
```

**🔹 Why SSG?**
- **Very fast** (data is prebuilt).
- Best for **static content** (e.g., blogs, product pages).

---

## 📌 **7. Using GraphQL in Next.js API Routes**
Sometimes, you may want your **Next.js API routes** to proxy GraphQL requests.

### 🔹 **Example: Creating an API Route**
Create a file: `app/api/graphql/route.ts`
```ts
import { NextResponse } from "next/server";
import { request, gql } from "graphql-request";

const GET_USERS = gql`
  query {
    users {
      id
      name
      email
    }
  }
`;

export async function GET() {
  try {
    const data = await request("https://your-graphql-api.com/graphql", GET_USERS);
    return NextResponse.json(data);
  } catch (error) {
    return NextResponse.json({ error: "Failed to fetch data" }, { status: 500 });
  }
}
```

Now, you can call:
```sh
GET /api/graphql
```

**🔹 Why use API Routes?**
- **Hides API keys** from frontend.
- **Adds middleware (Auth, Logging, etc.)** before calling GraphQL.

---

## 🎯 **Final Summary**
| Fetching Method | Library | Component Type | Best For |
|----------------|---------|---------------|----------|
| Apollo Client (`useQuery`) | `@apollo/client` | **Client** | Interactive UI, Auto caching |
| `graphql-request` | `graphql-request` | **Server** | Fast & lightweight API calls |
| SSR (`getServerSideProps`) | `graphql-request` | **Server** | Real-time & dynamic content |
| SSG (`getStaticProps`) | `graphql-request` | **Server** | Static, SEO-friendly content |
| API Routes | `graphql-request` | **Server** | Middleware, Securing API calls |

---

### 🔥 **Which Should You Use?**
- **For dynamic, user-interactive UI** → Use **Apollo Client** (`useQuery`).
- **For SEO & fast static pages** → Use **SSG (`getStaticProps`)**.
- **For real-time data (e.g., dashboard)** → Use **SSR (`getServerSideProps`)**.
- **For hiding GraphQL API calls** → Use **API Routes**.
