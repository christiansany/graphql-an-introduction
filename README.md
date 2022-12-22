# GraphQL an Introduction

> Disclaimer: This Introduction to GraphQL was done in cooperation with [ChatGPT](https://chat.openai.com/chat)

## Contents

1. What is GraphQL
2. Schema and Types
3. Queries and Mutations

TODO: What does this introduction cover
TODO: Move what is grpahql to the first page

## What is GraphQL

TODO: Image of graphql

GraphQL is a query language for your API that allows you to request specific data from an API, rather than getting a fixed set of data from an endpoint. It was developed by Facebook and is often used as an alternative to REST APIs.

With GraphQL, you define a schema for your API that describes the types of data that can be queried, as well as the relationships between those types. You can then use the GraphQL query language to request specific data from the API by sending a query to the API endpoint. The API will then return the requested data in a predictable, organized way.

One of the benefits of using GraphQL is that it allows you to request exactly the data that you need, rather than getting a fixed set of data from an endpoint. This can make it more efficient to work with APIs, especially if you only need a small amount of data from a large API. Additionally, GraphQL provides a more flexible way to work with APIs, as you can request and receive nested data using a single query.

### GraphQL vs REST

They are designed to allow client applications to communicate with servers and access data, but they do so in different ways. Here are some key differences between GraphQL and REST:

- Structure: REST APIs have a fixed structure, with a set of endpoints that return a fixed set of data when called. In contrast, GraphQL allows the client to request specific data by defining a query and sending it to the server. The server then returns the requested data.
- Flexibility: Because GraphQL allows the client to request specific data, it can be more flexible than REST. With REST, the client must use the endpoints provided by the API, which may not always provide the exact data needed. With GraphQL, the client can request exactly the data it needs, and receive it in a predictable, organized way.
- Overfetching and underfetching: REST APIs can sometimes suffer from overfetching or underfetching data. Overfetching occurs when the API returns more data than the client needs, which can result in slower performance and unnecessary network usage. Underfetching occurs when the client needs to make multiple API calls to get all of the data it needs, which can also impact performance. GraphQL can help mitigate these issues by allowing the client to request only the data it needs in a single query.
- Versioning: REST APIs often require versioning when changes are made to the API, as older versions of the API may still be in use by client applications. In contrast, GraphQL allows the server to make changes to the API without requiring versioning, as the client can request specific data and the server can return it in the format required.

Both GraphQL and REST have their own strengths and use cases, and the choice between them will depend on the needs of your specific project.

## Contents

TODO: What is this introduction containing???
TODO: Introduction as to how we want to learn -> We're focusing on the server part
TODO: Mini Exampel of Hello World

<div style="display: flex; justify-content: space-between;">
<span></span>
<a href="./schema-and-types.md">Next: TODO Title next 👉</a>
</div>
