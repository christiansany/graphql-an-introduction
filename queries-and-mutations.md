# Queries and Mutations

Here is a simple example of a GraphQL query that returns the message "Hello, world!":

```gql
query {
  hello
}
```

This query has a single field, `hello`, which requests the value of the
`hello` field from the server.

To execute this query, you will need to define a GraphQL schema that includes
a `Query` type with a `hello` field. Here is an example of a GraphQL schema
that defines a `Query` type with a `hello` field:

```gql
type Query {
  hello: String
}
```
