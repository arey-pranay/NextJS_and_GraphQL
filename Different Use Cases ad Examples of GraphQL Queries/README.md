### 📝 **Understanding GraphQL with Complete API Data**  

Before diving into queries, let's first **define a complete dataset** that we’ll be using for GraphQL queries, mutations, filters, pagination, etc.  

---

## 📌 **1. Sample API Data**  
Imagine we have a **GraphQL API** for a **blogging platform** with `Users`, `Posts`, and `Comments`. Here’s the **entire dataset** that the API holds:

```json
{
  "users": [
    {
      "id": "1",
      "name": "Pranay",
      "email": "pranay@example.com",
      "age": 22,
      "address": { "city": "Bangalore", "country": "India" },
      "posts": [
        { "id": "101", "title": "GraphQL Basics", "content": "GraphQL is awesome!" },
        { "id": "102", "title": "Next.js and GraphQL", "content": "Best combo!" }
      ]
    },
    {
      "id": "2",
      "name": "Aditi",
      "email": "aditi@example.com",
      "age": 24,
      "address": { "city": "Mumbai", "country": "India" },
      "posts": [
        { "id": "103", "title": "Understanding APIs", "content": "APIs power the web!" }
      ]
    }
  ],
  "posts": [
    {
      "id": "101",
      "title": "GraphQL Basics",
      "content": "GraphQL is awesome!",
      "author": "Pranay",
      "comments": [
        { "id": "201", "text": "Great post!", "author": "Aditi" },
        { "id": "202", "text": "Really helpful!", "author": "Rahul" }
      ]
    },
    {
      "id": "102",
      "title": "Next.js and GraphQL",
      "content": "Best combo!",
      "author": "Pranay",
      "comments": []
    },
    {
      "id": "103",
      "title": "Understanding APIs",
      "content": "APIs power the web!",
      "author": "Aditi",
      "comments": [
        { "id": "203", "text": "Nice explanation!", "author": "Pranay" }
      ]
    }
  ]
}
```

Now that we have the data, let's move to **GraphQL queries!** 🚀  

---

## 📌 **2. Basic GraphQL Query (Fetching Everything)**  
Let's start by **fetching all users with their posts and comments**.

```graphql
query {
  users {
    id
    name
    email
    age
    address {
      city
      country
    }
    posts {
      id
      title
      content
      comments {
        id
        text
        author
      }
    }
  }
}
```

✅ **Response:**  
```json
{
  "data": {
    "users": [
      {
        "id": "1",
        "name": "Pranay",
        "email": "pranay@example.com",
        "age": 22,
        "address": { "city": "Bangalore", "country": "India" },
        "posts": [
          {
            "id": "101",
            "title": "GraphQL Basics",
            "content": "GraphQL is awesome!",
            "comments": [
              { "id": "201", "text": "Great post!", "author": "Aditi" },
              { "id": "202", "text": "Really helpful!", "author": "Rahul" }
            ]
          },
          {
            "id": "102",
            "title": "Next.js and GraphQL",
            "content": "Best combo!",
            "comments": []
          }
        ]
      },
      {
        "id": "2",
        "name": "Aditi",
        "email": "aditi@example.com",
        "age": 24,
        "address": { "city": "Mumbai", "country": "India" },
        "posts": [
          {
            "id": "103",
            "title": "Understanding APIs",
            "content": "APIs power the web!",
            "comments": [
              { "id": "203", "text": "Nice explanation!", "author": "Pranay" }
            ]
          }
        ]
      }
    ]
  }
}
```

---

## 📌 **3. Filtering Data with Arguments**  
Let's **fetch a single user by ID**.

```graphql
query {
  user(id: "1") {
    name
    email
    posts {
      title
      content
    }
  }
}
```

✅ **Response:**  
```json
{
  "data": {
    "user": {
      "name": "Pranay",
      "email": "pranay@example.com",
      "posts": [
        { "title": "GraphQL Basics", "content": "GraphQL is awesome!" },
        { "title": "Next.js and GraphQL", "content": "Best combo!" }
      ]
    }
  }
}
```

---

## 📌 **4. Pagination (`first` & `after`)**
Fetching **first 2 users**.

```graphql
query {
  users(first: 2) {
    id
    name
  }
}
```

✅ **Response:**  
```json
{
  "data": {
    "users": [
      { "id": "1", "name": "Pranay" },
      { "id": "2", "name": "Aditi" }
    ]
  }
}
```

---

## 📌 **5. Mutations (Adding a New User)**
Mutation to **add a new user**.

```graphql
mutation {
  createUser(name: "Rahul", email: "rahul@example.com", age: 23) {
    id
    name
    email
  }
}
```

✅ **Response:**
```json
{
  "data": {
    "createUser": {
      "id": "3",
      "name": "Rahul",
      "email": "rahul@example.com"
    }
  }
}
```

---

## 📌 **6. Real-Time Updates (Subscriptions)**
Listen for **new posts being added**.

```graphql
subscription {
  newPost {
    id
    title
    content
    author
  }
}
```

✅ **When a new post is created, the client gets:**  
```json
{
  "data": {
    "newPost": {
      "id": "104",
      "title": "Learning GraphQL Subscriptions",
      "content": "This is real-time data!",
      "author": "Rahul"
    }
  }
}
```

---

## 🎯 **Final Thoughts**
Now you have:  
✅ **Full API Data** ✅ **Basic Queries** ✅ **Filters & Pagination** ✅ **Mutations** ✅ **Real-Time Subscriptions**  

