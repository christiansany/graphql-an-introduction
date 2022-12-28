# Schema and Types

A GraphQL schema is a blueprint for a GraphQL API. It defines the types of
data that can be queried, as well as the relationships between those types. It
also defines the root types for queries (`Query`) and mutations (`Mutation`),
which are the entry points for reading and writing data in the API.

A GraphQL schema is written in the GraphQL schema language, which is a
language specifically designed for defining GraphQL schemas. It is composed
of types, fields, and directives, which are used to define the shape of the data
that can be queried in the API.

Here is an example of a GraphQL schema:

```graphql
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

Here is an example of a GraphQL object type:

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

### Query Type

In GraphQL, the Query type represents a read operation. It defines a set of fields that can be queried to retrieve data from the server.

For example, consider a simple GraphQL schema that represents a list of users, where each user has a name and an email address:

```gql
type Query {
  users: [User]
}

type User {
  name: String
  email: String
}
```

In this schema, the `users` field on the `Query` type is a list of `User`
objects, which have `name` and `email` fields. To retrieve the list of users, a
client could send the following query:

```gql
query {
  users {
    name
    email
  }
}
```

The server would then respond with a list of users, each with a `name` and an `email` field.

**Demo 🤯**

TODO: Demo

### Mutation Type

In GraphQL, the Mutation type represents a write operation. It defines a set of fields that can be used to create, update, or delete data on the server.

For example, consider a simple GraphQL schema that represents a list of users, where each user has a name and an email address:

```gql
type Mutation {
  userAdd(name: String, email: String): User
  userUpdate(id: ID, name: String, email: String): User
  userDelete(id: ID): User
}

type User {
  id: ID
  name: String
  email: String
}
```

In this schema, the `userAdd`, `userUpdate`, and `userDelete` fields on the
`Mutation` type allow clients to create, update, and delete `User` objects,
respectively. To add a new user, a client could send the following mutation:

```gql
mutation {
  userAdd(name: "Alice", email: "alice@example.com") {
    id
    name
    email
  }
}
```

The server would then create a new `User` object with the specified name and email, and return the object with its `id` field, as well as the `name` and `email` fields.

**Demo 🤯**

TODO Demo

## Scalar Types

GraphQL scalar types represent leaf values in the GraphQL type system. They can be one of the following built-in scalar types: `Int`, `Float`, `String`, `Boolean`, or `ID`, or they can be a custom scalar type defined by the API. Scalar types are used to represent single values, such as numbers or strings, rather than complex objects or collections of data.

Here is an example of a GraphQL scalar types:

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

Here is an example of a GraphQL enumeration types:

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

Nullability refers to whether a field can return a null value or not. By default, fields in GraphQL are nullable, which means they can return a null value if no data is available.

For example, consider the following GraphQL schema:

```gql
type User {
  id: ID!
  name: String
  email: String
}

type Query {
  user(id: ID!): User
}
```

In this schema, the `user` field on the `Query` type returns a `User` object and is nullable.
This means that if a `user` is not found with the given `id`, the `user`
field will return null.

To make a field non-nullable, you can add an exclamation mark (!) after the
type. For example, the `id` field in the `User` object is non-nullable, because it
has a `!` after the `ID` type. This means, when a `User` is returned, it will always have an `id`.

In practice, it is often useful to make fields non-nullable when they are
required to perform an operation or when you want to ensure that a field
always has a value.

**Demo 🤯**

TODO Demo

## Lists

In GraphQL, a list is an ordered collection of values of the same type. Lists are represented in GraphQL using square brackets (`[]`).

For example, consider the following GraphQL schema:

```gql
type User {
  name: String
  email: String
}

type Query {
  users: [User]
}
```

In this schema, the `users` field on the `Query` type returns a list of users. The list is nullable, this mean that it would be valid for the `users` field to return `null`, or an array or vallues in which the values also can be `null` (`[null, null, null]`). In order to make a list non-nullable, we have to add a `!` after the Type and after the list.

```gql
type User {
  name: String
  email: String
}

type Query {
  users: [User!]!
}
```

**Demo 🤯**

[Nullable Book List](https://stackblitz.com/edit/graphql-phydnd?file=src%2Findex.ts)

## Interfaces

An interface is a type that defines a set of fields that other types can implement.
Interfaces allow you to create a common set of fields that can be shared among different types,
while still allowing those types to have their own unique fields.

For example, consider the following GraphQL schema:

```gql
interface Node {
  id: ID!
}

type User implements Node {
  id: ID!
  name: String
  email: String
}

type Post implements Node {
  id: ID!
  title: String
  body: String
  author: User
}

type Query {
  node(id: ID!): Node
}
```

In this schema, the `Node` interface defines a single field, `id`, which is a
non-nullable `ID`. Both the `User` and `Post` types implement the `Node`
interface, which means they must include an `id` field. The `User` and
`Post` types also have their own unique fields, such as `name`, `email`,
`title`, and `body`.

To retrieve a `Node` object, a client can use the `node` field on the `Query`
type, and pass in the `id` of the object they want to retrieve. The server will
then return the appropriate object, either a `User` or a `Post`, with all of its
fields, including the `id` field inherited from the `Node` interface.

## Union Types

A union type is a type that represents a set of other types. A union
type allows you to define a common set of fields that can be shared among
different types, while still allowing those types to have their own unique
fields.

For example, consider the following GraphQL schema:

```gql
type User {
  id: ID!
  name: String
  email: String
}

type Post {
  id: ID!
  title: String
  body: String
}

union SearchResult = User | Post

type Query {
  search(query: String!): [SearchResult]
}
```

In this schema, the `SearchResult` union type represents either a `User` or a
`Post` object. The `search` field on the `Query` type returns a list of
`SearchResult` objects, which means it can return a list of either `User` or
`Post` objects, or a mix of both.

To query the list of search results, a client could send the following query:

```gql
query {
  search(query: "Alice") {
    __typename
    ... on User {
      name
      email
    }
    ... on Post {
      title
      body
    }
  }
}
```

The server would then return a list of search results, each with a
`__typename` field that indicates the type of the object (either `User` or
`Post`). The client can use the `... on` syntax to specify which fields to
return for each type. On on `... on` in the fragments section later.

## Input Types

In GraphQL, an input type is a type that represents data that can be passed as
an argument to a field in a GraphQL query or mutation. Input types are used to
specify the data that a field expects as input, and they are defined using the
input keyword.

For example, consider the following GraphQL schema:

```gql
input UserCreateInput {
  name: String
  email: String
}

type Mutation {
  userCreate(input: UserCreateInput!): User
}

type User {
  id: ID!
  name: String
  email: String
}
```

In this schema, the `UserCreateInput` input type represents the data that the
`userCreate` field expects as input. The `userCreate` field takes a single
argument, `input`, which is a non-nullable `UserCreateInput` object. The
`UserCreateInput` object has two fields, `name` and `email`, both of which are
nullable strings.

To create a new user, a client could send the following mutation:

```gql
mutation {
  userCreate(input: { name: "Alice", email: "alice@example.com" }) {
    id
    name
    email
  }
}
```

The server would then create a new `User` object with the specified name and email,
and return the object with its `id` field, as well as the `name` and `email` fields.

**it's a best practice to always use input types for mutations**.

[👉 Next: Queries and Mutations](./queries-and-mutations.md)
