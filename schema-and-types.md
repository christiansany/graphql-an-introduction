# Schema and Types

A GraphQL schema is a blueprint for a GraphQL API. It defines the types of
data that can be queried, as well as the relationships between those types. It
also defines the root types for queries (`Query`) and mutations (`Mutation`),
which are the entry points for reading and writing data in the API.

A GraphQL schema is written in the GraphQL schema language, which is a
language specifically designed for defining GraphQL schemas. It is composed
of types, fields, and directives, which are used to define the shape of the data
that can be queried in the API.

**Here is an example of a GraphQL schema:**

```gql
type Query {
  users: [User]
  user(id: ID!): User
}

type Mutation {
  userCreate(name: String!, email: String!): User
  userUpdate(id: ID!, name: String, email: String): User
  userDelete(id: ID!): User
}

type User {
  id: ID!
  name: String!
  email: String!
  posts: [Post]
}

type Post {
  id: ID!
  title: String!
  body: String!
  author: User!
}
```

In this example, the schema defines a Query type with two fields: users and user. The users field returns an array of User objects, while the user field returns a single User object based on its id. The schema also defines a Mutation type with three fields: userCreate, userUpdate, and userDelete, which allow you to create, update, and delete User objects in the API. The User and Post types define the shape of the User and Post data, respectively.

You can use a GraphQL schema to validate and execute GraphQL queries and mutations against your data. It serves as a contract between the client and server, defining the types of data that can be queried and the operations that can be performed on that data.

## Object Types

GraphQL object types represent a group of related fields in a GraphQL schema. They define the shape of the data that can be queried in a GraphQL API, and are composed of fields, each of which has a name and a type. Object types can contain both scalar fields, which represent single values, and object fields, which represent nested objects. They are used to define complex data structures and relationships between different types of data in a GraphQL API.

**Here is an example of a GraphQL object type:**

```gql
type User {
  id: ID!
  name: String!
  email: String!
  posts: [Post]
}
```

<details>
  <summary>More about the example</summary>
  
This object type represents a `User` in a GraphQL API. It has four fields: `id`,
`name`, `email`, and `posts`. The `id` field is of type `ID`, which is a non-nullable scalar type that represents a unique identifier. The `name` and
`email` fields are of type `String`, and are marked with an exclamation point
(`!`) to indicate that they are non-nullable. The `posts` field is an array of
`Post` objects.
</details>

## The Query and Mutation Type

TODO

## Scalar Types

GraphQL scalar types represent leaf values in the GraphQL type system. They can be one of the following built-in scalar types: `Int`, `Float`, `String`, `Boolean`, or `ID`, or they can be a custom scalar type defined by the API. Scalar types are used to represent single values, such as numbers or strings, rather than complex objects or collections of data.

**Here is an example of a GraphQL scalar types:**

```gql
type User {
  id: ID!
  name: String!
  email: String!
  age: Int
  isActive: Boolean
}
```

<details>
  <summary>More about the example</summary>
  
This object type represents a `User` in a GraphQL API. It has five fields: `id`,
`name`, `email`, `age`, and `isActive`. The `id` field is of type `ID`, which
is a non-nullable scalar type that represents a unique identifier. The `name`
and `email` fields are of type `String`, and are marked with an exclamation
point (`!`) to indicate that they are non-nullable. The `age` field is of type
`Int`, which represents an integer value, and the `isActive` field is of type
`Boolean`, which represents a boolean value (`true` or `false`).
</details>

## Enumeration Types

GraphQL enumeration types represent a fixed set of values in a GraphQL schema. They are used to define a type that can have one of a limited set of predefined values, each of which is associated with a unique name. Enumeration types are often used to represent a set of options or states that a field can have, such as the status of a task (e.g., "pending", "in progress", or "completed").

**Here is an example of a GraphQL enumeration types:**

```gql
enum TaskStatus {
  PENDING
  IN_PROGRESS
  COMPLETED
}
```

In this example, the `TaskStatus` enumeration type has three values:
`PENDING`, `IN_PROGRESS`, and `COMPLETED`. You can use this enumeration
type to define a field in a GraphQL object type that can have one of these
values, like this:

```gql
type Task {
  id: ID!
  title: String!
  status: TaskStatus!
}
```

The `status` field in this object type is of type `TaskStatus`, and can have one of the three predefined values.

## Nullable and Non-Nullable

TODO

## Lists

TODO

## Interfaces

TODO -> Later, not relevant for the quick intro
https://graphql.org/learn/schema/#interfaces

## Union Types

TODO -> Later, not relevant for the quick intro
https://graphql.org/learn/schema/#union-types

## Input Types

TODO -> Later, not relevant for the quick intro
https://graphql.org/learn/schema/#input-types
