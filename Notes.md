## Schema First Design

- Defining the schema: We identify which data our feature requires, and then we structure our schema to provide that data as intuitively as possible.
- Backend implementation: We build out our GraphQL API using Apollo Server and fetch the required data from whichever data sources contain it. In this first course, we will be using mocked data. In a following course, we'll connect our app to a live REST data source.
- Frontend implementation: Our client consumes data from our GraphQL API to render its views.


### Mockup by Design Team

![Mockup](https://res.cloudinary.com/apollographql/image/upload/e_sharpen:50,c_scale,q_90,w_1440,fl_progressive/v1612409047/odyssey/lift-off-part1/mockup_rlr38m_dt4zrt.jpg)

### Data Graph (based on attributes in Mockup)

![Data Graph](https://res.cloudinary.com/apollographql/image/upload/e_sharpen:50,c_scale,q_90,w_1440,fl_progressive/v1612409160/odyssey/lift-off-part1/LO_02_v2.00_04_53_09.Still002_g8xow6_bbgabz.jpg)

### GraphQL Schema Walkthrough

A schema is like a contract between the server and the client. It defines what a GraphQL API can and can't do, and how clients can request or change data. It's an abstraction layer that provides flexibility to consumers while hiding backend implementation details.

![Schema](https://res.cloudinary.com/apollographql/image/upload/e_sharpen:50,c_scale,q_90,w_1440,fl_progressive/v1612409235/odyssey/lift-off-part1/type_spacecat_aymp3y_l04j48.jpg)

For the purpose of Documenting the Schema, it is a good practice to add descriptions to both types and fields by writing strings (in quotation marks) directly above them.

`
"I'm a regular description"
`

`
"""
I'm a block description
with a line break
"""
`

### Apollo Provider and Client

Apollo Client is an open-source library for client-side state management and GraphQL operation handling in Javascript/Typescript.

The ApolloProvider component uses React's Context API to make a configured Apollo Client instance available throughout a React component tree. To use it, we wrap our app's top-level components in the ApolloProvider component and pass it our client instance as a prop

### Best practices when creating client queries

- Wrap each query in the `gql` function
- Include only the fields that the client requires.
- Assign each query string to a constant with an ALL_CAPS name
- Test out queries in the Apollo Studio Explorer and copy them over




## Miscellaneous

### Common Packages related to GraphQL

- The `@apollo/server` package provides a full-fledged, spec-compliant GraphQL server.
- The `@apollo/client` package contains pretty much everything we need to build our client, including an in-memory cache, local state management, and error handling.
- The `graphql` package provides the core logic for parsing and validating GraphQL queries.
- The `graphql-tag` package provides the gql template literal, used for wrapping GraphQL strings
- The `@graphql-tools/mock` package provides a way to return mock data for testing purposes
- The `@graphql-tools/schema` package allows us to create executable schema from our typeDefs

### Mocking data when you don't yet have a data source

```javascript
const mockData = {
    a: b,
    c: 'd'
    e: { f: g }
};
const server = new ApolloServer({
  schema: addMocksToSchema({
    schema: makeExecutableSchema({ typeDefs }),
    mockData
  })
});
```

### Keeping the Types consistent b/w Backend and Frontend

We need the frontend to understand what type of schema exists on the Backend to write effective queries. We could write out the TypeScript types manually but if we change our schema in the future, we have to remember to update our frontend as well; This means that our frontend's TypeScript types can easily get out of sync, if we're not careful!

Instead, we can look to the GraphQL API's schema as the "single source of truth" for all of the types we could possibly query on the frontend. An easy way to do this, and to keep our frontend's type definitions consistent with the backend, is to use a GraphQL Code Generator (like `@graphql-codegen/cli`).

Here's how the process works.
- Install `@graphql-codegen/cli` and `@graphql-codegen/client-preset`, and set up a generate command to launch the Code Generator.
- Next, create a configuration file that guides the codegen process. At its most basic, this specifies the path to the GraphQL schema, the files in our frontend app it should inspect for GraphQL operations, and where to output the necessary TypeScript types.
- Run our codegen command.
- Finally, check out the types it generated!