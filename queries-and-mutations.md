# Queries and Mutations

## Queries

In GraphQL, a client sends queries to a server to retrieve data. Queries are
written in the GraphQL query language, and they consist of a set of fields that
the client wants to retrieve from the server.

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

To retrieve a list of users from the server, a client could send the following query:

```gql
query {
  users {
    name
    email
  }
}
```

This query specifies that the client wants to retrieve the `users` field from the
`Query` type, and that it wants the `name` and `email` fields for each user in
the list. The client accesses data always through the `Query` type.

The server would then execute the query and return the requested data in the
form of a JSON object. For example:

```json
{
  "data": {
    "users": [
      {
        "name": "Alice",
        "email": "alice@example.com"
      },
      {
        "name": "Bob",
        "email": "bob@example.com"
      }
    ]
  }
}
```

Queries can also include arguments and variables to specify additional information
that the server needs to execute the query. For example:

```gql
query GetUser($id: ID!) {
  user(id: $id) {
    name
    email
  }
}
```

In this query, the `user` field takes an `id` argument, which is passed using a
variable named `$id`. To execute this query, the client would need to pass a value
for the `$id` variable when making the request. More on [arguments](#arguments) later.

Queries are a fundamental part of GraphQL, and they are used by clients to retrieve
data from a GraphQL server.

**Demo 🤯**

TODO Demo

## Mutations

In GraphQL, a client sends mutations to a server to create, update, or delete
data. Mutations are written in the GraphQL query language, and they consist of a
set of fields that the client wants to modify on the server.

For example, consider the following GraphQL schema:

```gql
type User {
  name: String
  email: String
}

input UserCreateInput {
  name: String!
  email: String!
}

input UserUpdateInput {
  id: ID!
  name: String
  email: String
}

input UserDeleteInput {
  id: ID!
}

type Mutation {
  userCreate(input: UserCreateInput!): User
  userUpdate(input: UserUpdateInput!): User
  userDelete(input: UserDeleteInput!): User
}
```

In this schema, the `Mutation` type has three fields: `userCreate`, `userUpdate`,
and `userDelete`. These fields allow the client to create, update, and delete
`User` objects on the server, respectively.

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

This mutation specifies that the client wants to execute the `userCreate`
field on the `Mutation` type, and that it wants to pass the `input` argument to
the field. The server would then create a new `User`
object with the specified name and email in the `input`, and return the object with the
`name` and `email` fields.

To update an existing user, the client could send a similar mutation, but using
the `userUpdate` field and passing an `input` argument that also requires the `id`
of the user we want to change:

```gql
mutation {
  userUpdate(input: { id: "123", name: "Alice", email: "alice@example.com" }) {
    id
    name
    email
  }
}
```

To delete a user, the client could send a mutation using the `userDelete` field
and passing in the `id` of the user to be deleted:

```gql
mutation {
  userDelete(input: { id: "123" }) {
    id
    name
    email
  }
}
```

**Demo 🤯**

TODO Demo

## Arguments

In GraphQL, arguments are values that are passed to a field in a GraphQL query
or mutation. Arguments allow the client to specify additional information that
the server needs to execute the query or mutation.

For example, consider the following GraphQL schema:

```gql
type Query {
  user(id: ID!): User
}

type User {
  name: String
  email: String
  friends(first: Int): [User]
}
```

In this schema, the `user` field on the `Query` type and the `friends`
field on the `User` type both take arguments. The `user` field takes a
single argument, `id`, which is a non-nullable ID. The `friends` field takes a
single argument `first` which is nullable.

To query a specific user, a client could send the following query:

```gql
query {
  user(id: "123") {
    name
    email
  }
}
```

## Variables

Variables are values that are passed to a query or mutation as a separate
argument, rather than being embedded directly in the query or mutation.
Variables allow the client to reuse the same query or mutation with different
values, and they are defined using the `$` symbol.

For example, consider the following GraphQL schema:

```gql
type Query {
  user(id: ID!): User
}

type User {
  name: String
  email: String
}
```

To query a specific user, a client could send the following query:

```gql
query GetUser($id: ID!) {
  user(id: $id) {
    name
    email
  }
}
```

This query specifies that the client wants to retrieve the `user` field from the
`Query` type, and that it wants to pass the `id` argument using a variable
named `$id`. The `$id` variable is defined as a non-nullable ID.

To execute this query, the client would need to pass a value for the `$id`
variable when making the request. For example:

```json
{
  "query": "query GetUser($id: ID!) { user(id: $id) { name email } }",
  "variables": {
    "id": "123"
  }
}
```

## Aliases

In GraphQL, aliases are used to give a different name to a field when it is being queried. Aliases are useful when a field needs to be queried multiple times with different arguments, or when the same field needs to be queried under different names in the same query.

For example, consider the following GraphQL schema:

```gql
type Query {
  user(id: ID!): User
}

type User {
  name: String
  email: String
}
```

If the client wants to query the same fields with different arguments, or if it wants to query the same field under different names, it can use aliases.

For example, to query the name field for two different users, the client could use the following query:

```gql
query {
  user1: user(id: "123") {
    name
  }
  user2: user(id: "456") {
    name
  }
}
```

The server would execute the query by fetching the `user` field with the `id` argument set to `123` and `456`, respectively. The server would then return the data in the following format:

```json
{
  "data": {
    "user1": {
      "name": "Alice"
    },
    "user2": {
      "name": "Bob"
    }
  }
}
```

The `data` object contains the result of the query, with the `user1` and `user2` fields representing the data for the two different users. Each field contains the `name` field for the corresponding user.

## Fragments

Fragments are used to represent a set of fields that can be reused in multiple queries or mutations. Fragments allow the **client** to define a reusable piece of a query or mutation, and to include it in multiple places within the same request.

For example, look at the following query:

```gql
query {
  user1: user(id: "123") {
    ...UserFields
  }
  user2: user(id: "456") {
    ...UserFields
  }
}

fragment UserFields on User {
  name
  age
}
```

In this example the fragment `UserFields` is used for both user fields.

### Inline Fragments

Inline fragments are used to include a set of fields in a query or mutation that apply to a specific type. Inline fragments allow the client to include fields from a specific type within a union or interface type, without knowing the exact type in advance.

For example, consider the following GraphQL schema:

```gql
type Query {
  users: [User]
}

union User = Admin | Customer

type Admin {
  name: String
  email: String
  permissions: [String]
}

type Customer {
  name: String
  email: String
}
```

In the example, the `User` type is a union type. When we fetch our users, we can
get back either `Admin` or `Customer` types. To only access the fields that
we're alowed, our query msut be structured correctly.

```gql
query {
  users {
    ... on Admin {
      name
      email
      permissions
    }
    ... on Customer {
      name
      email
    }
  }
}
```

This query uses inline fragments to include the `name` and `email` fields for both the `Admin` and `Customer` types. The `... on Admin` and `... on Customer` parts specify that the fields should be included only if the object is of the corresponding type.

## Directives

Directives are used to modify the execution of a query or mutation. Directives
allow the client to include additional information in the query or mutation
that tells the server how to execute the request.

For example, have a look at the following query:

```gql
query {
  users {
    name
    age @skip(if: age < 18)
  }
}
```

With the `@skip` directive, we can tell the server, that this field should be skipped if the condition is met that is described in the directive.

Note: By default the `@skip`, `@include` and the `@deprecated` directives exist, but we can add custom directives ourselfs.

[👉 Next: Apollo Server](./apollo-server.md)
