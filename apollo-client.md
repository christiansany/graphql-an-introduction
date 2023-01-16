# Apollo Client

## Getting Started

### 1. Installing the required packages

```bash
npm install @apollo/client graphql
```

When installing packages for the [Spotify Clone](https://github.com/christiansany/spotify-clone)
Monorepo, make sure to install them inside the correct package and not globally:

```bash
npm install @apollo/client graphql --workspace=web
```

### 2. Creating your client instance

Now we're creating our `ApolloClient` instance and also creating an instance of
`InMemoryCache` to provide our client with a cache.

```ts
// apollo-client.ts
import { ApolloClient, InMemoryCache } from "@apollo/client";

export const client = new ApolloClient({
  uri: "http://localhost:4000",
  cache: new InMemoryCache(),
});
```

In the [Spotify Clone](https://github.com/christiansany/spotify-clone), this file
already exists and already includes `possibleTypes` which tells our `InMemoryCache`
what our `union` Types are in our GraphQL Schema. We generate thos with the
`npm run codegen` command.

### 3. Executing your first query

To have a good DX, we're using the new [client preset](https://the-guild.dev/graphql/codegen/plugins/presets/preset-client)
of [GraphQL Code Generator](https://the-guild.dev/graphql/codegen). This generates
TypeScript types for our operations by using the `graphql()` function. Keep in mind that as long as the types for your operations are not generated, the TypeScript compiler will tell you taht your code has errors. So it's best to have the code generator always running with running `npm run codegen:watch`.

```ts
import { graphql } from "../__generated__/gql";
import { client } from "./apollo-client";

const GetSongDocument = graphql(`
  query GetSong {
    song(id: "song:1") {
      id
    }
  }
`);

client
  .query({
    query: GetSongDocument,
  })
  .then(({ data }) => {
    console.log(data);
  });
```

🎉 You've jsut made your first query.

### 4. Setup `ApolloProvider` to use hooks inside your components

Inside our `_app.tsx` we can register our client with the `ApolloProvider` like so:

```ts
import type { AppProps } from "next/app";
import { ApolloProvider } from "@apollo/client";
import { client } from "../src/apollo-client";

export default function MyApp({ Component, pageProps }: AppProps) {
  return (
    <ApolloProvider client={client}>
      <Component {...pageProps} />
    </ApolloProvider>
  );
}
```

After adding our `ApolloProvider` we can then use our hooks `useQuery()`, `useLazyQuery()`
and `useMutation()` inside our function components.

### 5. Creating your first FunctionComponent that uses hooks

Creat a FunctionComponent that fetches the `name` of a perticular song and that songs artists `name`.

```tsx
import { useQuery } from "@apollo/client";
import React, { FunctionComponent } from "react";
import { graphql } from "../../__generated__/gql";

const GetSongDetailsDocument = graphql(`
  query GetSongDetails {
    song(id: "song:1") {
      id
      name
      artist {
        id
        name
      }
    }
  }
`);

export const SongDetails: FunctionComponent = () => {
  const { data, loading, error } = useQuery(GetSongDetailsDocument);

  if (loading) return <h1>Loading...</h1>;
  if (error) return <h1>Error: {error.message}</h1>;

  return (
    <>
      <h1>Song: {data?.song?.name}</h1>
      <p>Artist: {data?.song?.artist?.name}</p>
    </>
  );
};
```

Display this component on your page `pages/index.ts`.  
🎉 You've just made a query inside of your FunctionComponent.

## Queries

### Variables

We can also provide variables in our queries:

```tsx
import { useQuery } from "@apollo/client";
import React, { FunctionComponent } from "react";
import { graphql } from "../../__generated__/gql";

const GetSongDetailsDocument = graphql(`
  query GetSongDetails($id: ID!) {
    song(id: $id) {
      id
      name
      artist {
        id
        name
      }
    }
  }
`);

export const SongDetails: FunctionComponent = () => {
  const { data, loading, error } = useQuery(GetSongDetailsDocument, {
    variables: {
      id: "song:1",
    },
  });

  if (loading) return <h1>Loading...</h1>;
  if (error) return <h1>Error: {error.message}</h1>;

  return (
    <>
      <h1>Song: {data?.song?.name}</h1>
      <p>Artist: {data?.song?.artist?.name}</p>
    </>
  );
};
```

### Pagination

When loading lists, often we don't want all entries at the same time, but load
them in chunks (pages).

```tsx
import { useQuery } from "@apollo/client";
import React, { FunctionComponent } from "react";
import { graphql } from "../../__generated__/gql";

const GetSongsDocument = graphql(`
  query GetSongs($take: Int!, $skip: Int) {
    songs(take: $take, skip: $skip) {
      id
      name
    }
  }
`);

export const SongsList: FunctionComponent = () => {
  const { data, loading, error, fetchMore } = useQuery(GetSongsDocument, {
    variables: { take: 5 },
  });

  if (loading) return <h1>Loading...</h1>;
  if (error) return <h1>Error: {error.message}</h1>;

  return (
    <>
      <h1>SongsList</h1>
      <ul>
        {data?.songs.map((song) => (
          <li>
            <p>Song: {song.name}</p>
          </li>
        ))}
      </ul>
      <button
        onClick={() => fetchMore({ variables: { skip: data?.songs.length } })}
      >
        Load More
      </button>
    </>
  );
};
```

When clicking the "Load More" button, it will load the next 5 entries. But the UI
doesn't update with the newly loaded entries. The Cache doesn't yet understand that
when using fetchMore, the data of the first call and all consecutive calls should
be merged together and then displayed.

To enable our cache to understand what we want, we have to implement a TypePolicy for our `InMemoryCache`.

```ts
import { ApolloClient, InMemoryCache } from "@apollo/client";
import { concatPagination } from "@apollo/client/utilities";
import fragmentTypes from "../__generated__/fragments";

export const client = new ApolloClient({
  uri: "http://localhost:4000",
  cache: new InMemoryCache({
    possibleTypes: fragmentTypes.possibleTypes,
    typePolicies: {
      Query: {
        fields: {
          songs: concatPagination(),
        },
      },
    },
  }),
});
```

On our `songs` field on the `Query` we can set a TypePolicy that concatinates our
data into a single array. We can use the helper `concatPagination()` that is
provided by `@apollo/client/utilities`. With that our Client now understand to merge
the previous data and the incoming data when pressing "Load More".

Now we only have the issue that the "Load More" button is still displayed when
everything is loaded. Unfortunately that is currently not possible with our Schema.

More about Apollos type policies in regards to pagination
[here](https://www.apollographql.com/docs/react/pagination/core-api/#defining-a-field-policy).

For more information, go visit the [Core PaginationAPI](https://www.apollographql.com/docs/react/pagination/core-api/)

### Fragments

A fragment is a reusable piece of a GraphQL query that can be used to fetch specific
fields from a type. It allows you to avoid duplicating fields in multiple queries,
and also makes it easier to update your queries if the schema changes.

A fragment is defined using the `fragment` keyword, followed by the name of the fragment,
the type it applies to, and the fields it should include. For example:

```gql
fragment ArtistDetail_Artist on Artist {
  id
  name
  image
}
```

In this fragment, we describe data of the Artist. Now we can use this fragment
to load the data in another query with spreading the the fragments information
into that query:

```gql
query {
  artists {
    ...ArtistDetail_Artist
  }
}
```

In our Frontend Development we can use fragments, so we can describe the exact
data that our components need. Take for example these two components:

```tsx
// ArtistDetails.tsx
import React, { FunctionComponent } from "react";
import { FragmentType, graphql, useFragment } from "../../__generated__/gql";

const ArtistDetail_Artist = graphql(`
  fragment ArtistDetail_Artist on Artist {
    id
    name
  }
`);

export interface IArtistDetailsProps {
  artist: FragmentType<typeof ArtistDetail_Artist>;
}

export const ArtistDetails: FunctionComponent<IArtistDetailsProps> = ({
  artist: artistData,
}) => {
  const artist = useFragment(ArtistDetail_Artist, artistData);
  return <p>Artist: {artist.name}</p>;
};
```

```tsx
// SongDetails.tsx
import { useQuery } from "@apollo/client";
import React, { FunctionComponent } from "react";
import { graphql } from "../../__generated__/gql";
import { ArtistDetails } from "./ArtistDetails";

const GetSongDetailsDocument = graphql(`
  query GetSongDetails {
    song(id: "song:1") {
      id
      name
      artist {
        ...ArtistDetail_Artist
      }
    }
  }
`);

export const SongDetails: FunctionComponent = () => {
  const { data, loading, error } = useQuery(GetSongDetailsDocument);

  if (loading) return <h1>Loading...</h1>;
  if (error) return <h1>Error: {error.message}</h1>;

  return (
    <>
      <h1>Song: {data?.song?.name}</h1>
      {data?.song?.artist ? (
        <ArtistDetails artist={data.song.artist}></ArtistDetails>
      ) : null}
    </>
  );
};
```

In this example, the `ArtistDetails` component uses a fragment to describe the Data
that it needs to render.
In the `SongDetails` component we then load the fragment in our query, therefor
sucessfully loading all necessary information for the `ArtistDetails` component
to load.

Now, when then data requirements for ArtistDetails change, for example also needing
the artists image, it can simply add the `image` field to its fragment and the query
that is using that fragment to load the data will then automatically load the image
information.

## Mutations

Here's an example of a login Mutation. The input in this case, are the login credentials
of `username` & `password`.
The server then resolves the `userLogin` field with the given input. In case there
is an error, the server will fill the `userErrors` array and leave the `user` and
`token` fields empty. In case of a successfull login, the server will leave the
`userErrors` array empty, and fill in the `user` and `token` fields.  
The `token` field can then be stored so it can be sent with future requests.

[Documentation on Apollo](https://www.apollographql.com/docs/react/data/mutations)

```gql
mutation UserLogin($input: UserLoginInput!) {
  userLogin(input: $input) {
    userErrors {
      field
      message
    }
    user {
      id
      username
    }
    token
  }
}
```

The whole example of the LoginForm component could look something like this:

```tsx
import { useMutation } from "@apollo/client";
import { FunctionComponent } from "react";
import { graphql } from "../../__generated__/gql";

const UserLoginDocumentDocument = graphql(`
  mutation UserLogin($input: UserLoginInput!) {
    userLogin(input: $input) {
      userErrors {
        field
        message
      }
      user {
        id
        username
      }
      token
    }
  }
`);

export const LoginForm: FunctionComponent = () => {
  const [login, { data, loading, error }] = useMutation(
    UserLoginDocumentDocument
  );

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error!</p>;

  const onSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const formData = new FormData(e.target as HTMLFormElement);
    login({
      variables: {
        input: {
          username: (formData.get("username") as string) || "",
          password: (formData.get("password") as string) || "",
        },
      },
    }).then(({ data }) => {
      // TODO: if successfull, save the token so it can be sent with every request
      // See https://www.apollographql.com/docs/react/api/link/introduction
      // See https://www.apollographql.com/docs/react/api/link/apollo-link-context
      console.log(data);
    });
  };

  return (
    <form onSubmit={onSubmit}>
      {data?.userLogin?.userErrors?.map((error) => (
        <p>
          {error.field.join(",")}: {error.message}
        </p>
      ))}
      <label>
        Username
        <input type="text" name="username" required />
      </label>
      <label>
        Password
        <input type="password" name="password" required />
      </label>
      <button type="submit">Log in</button>
    </form>
  );
};
```
