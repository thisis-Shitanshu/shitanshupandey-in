---
layout: post
title: "GraphQL + Apollo"
categories: GraphQL Apollo React.js
published: true
---

Quick Notes: GraphQL + Apollo.
Practical Implementation, click [here](https://github.com/thisis-Shitanshu/crwn-clothing-ecommerce-react-firebase-stripe/tree/W/GraphQL-Apollo).

# #Introduction
- It's a Server Language that wraps around existing DB or Server, that you can make request against in "the GraphQL way".
- To use GraphQL server, you wrap your REST Server into GraphQL Server.
- Instead of multiple endpoints, our GraphQL server exposes single endpoint: `/graphql`.
    - To make a request to this end point, you need to pass it either a `query` or `mutation`.
    - Keeps the focus only on the kind of data we are to recieve and avoiding the distraction of modifying the Front-end for multiple endpoints.
- Every value returned by GraphQL is either a Data Type with **value** or **null**.
    - Data sits under the name of the Query/Mutation we made.

# #Requests

- Query: Get Data

```js
query {
    collections {
        id
        title
        items {
            id
            name
            price
            imageUrl
        }
    }
}
```

- Query with Parameters:

```js
query {
    collection(id: "asdfghjklqwertyuiop") {
        title
    }
}
```

# #Apollo
- Library to intreact with the GraphQL Server to handle the local state.

## #Apollo Client
- This client make sure to cache data that we fetch.
- Client help manage the data.

- Steps:
    1. Instanciate the client itself.
        - `$ yarn add apollo-boost react-apollo graphql`

    ```js
    // index.js
    import React from 'react';
    import ReactDOM from 'react-dom';
    import { BrowserRouter } from 'react-router-dom';

    import { ApolloProvider } from 'react-apollo';
    // From apollo-boost only
    import { createHttpLink } from 'apollo-link-http';
    import { InMemoryCache } from 'apollo-cache-inmemory';
    import { ApolloClient, gql } from 'apollo-boost';

    import './index.css';
    import App from './App';

    const httpLink = createHttpLink({
    uri: 'https://crwn-clothing.com'
    });

    const cache = new InMemoryCache();

    const client = new ApolloClient({
    link: httpLink,
    cache
    });

    client.query({
    query:  gql`
        {
        collections {
            id
            title
            items {
                id
                name
                price
                imageUrl
            }
        }
        }
    `
    }).then(res => console.log(res));

    ReactDOM.render(
    <ApolloProvider client={client}>
        <BrowserRouter>
            <App />
        </BrowserRouter>
    </ApolloProvider>,
    document.getElementById('root')
    );
    ```

## #GraphQL vs Redux

<img src="{{ "/assets/img/GraphQLStatemanagement.png" }}" alt="GraphQLStatemanagement.png">

- Resolvers are functions with access to local cache that modify data or get data.
    - GraphQL have resolvers on Front-end and Back-end.

# #Apollo Local Data Query & Mutation
1. Instantiate local value:
    
    ```js
    // index.js
    import { typeDefs, resolvers } from  './graphql/resolvers';
    
    const client = new ApolloClient({
        link: httpLink,
        cache,
        typeDefs,
        resolvers
    });

    client.writeData({
        data: {
            cartHidden: true
        }
    })
    ```

1. Create resolvers:
    - Object we can pass to our client, that lets it know what values to resolve depending on what mutation or queries get called from client side.

    ```js
    // resolver
    import { gql } from 'apollo-boost';

    export const typeDefs = gql`
        extend type Mutation {
            ToggleCartHidden: Boolean!
        }
    `;

    const GET_CART_HIDDEN = gql`
        {
            cartHidden @client
        }
    `;

    export const resolvers = {
        Mutation: {
            toggleCartHidden: (_root, _args, { cache }) => {
                const { cartHidden } = cache.readQuery({
                    query: GET_CART_HIDDEN
                });

                cache.writeQuery({
                    query: GET_CART_HIDDEN,
                    data: { cartHidden: !cartHidden }
                });

                return !cartHidden;
            } 
        }
    };
    ```

1. Implement in Components Container

```js
import React from 'react';
import { Mutation } from 'react-apollo';
import { gql } from 'apollo-boost';

import CartIcon from './cart-icon.component';

const TOGGLE_CART_HIDDEN = gql`
    mutation ToggleCartHidden {
        toggleCartHidden @client
    }
`;

const CartIconContainer = () => (
    <Mutation mutation={TOGGLE_CART_HIDDEN}>
        {toggleCartHidden => <CartIcon toggleCartHidden={toggleCartHidden} />}
    </Mutation>
);

export default CartIconContainer;
```

## #Note:
1. `compose` not exported from `react-apollo`:
    - Solution: `import {flowRight as compose} from 'lodash';`
