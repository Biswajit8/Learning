
REST API might have an endpoint that returns all of the data for a user, even if the client app / developer only needs a few pieces of the data. This can be inefficient if the user has a lot of data. The developer could query the API for the user's name & email, but not their address or phone no. With GraphQL, in a request only the specific data is returned which is needed. 

In REST, the API exposes a set of resources, each with its own structure and endpoints. To get data from multiple resources (Title, Author, Publication in case of Book), we need to make multiple requests to different endpoints, which can be inefficient specially if the resources are tightly coupled. In GraphQL, we can request any data that is available on the server in a single request. We can also define the structure of the response so that we can only get those data which we need. GraphQL servers can also cache query results which can reduce the no of round trips between the client and server, which makes GraphQL more flexible and efficient than REST. 

Example:

In REST, the below requests will return the User and Post object.
```
GET /users/{userId}
GET /posts/{postId}
```

However, if we only need the User name and the Post title, we will need to make 2 differrent requests. 

In GraphQL, we need a single query to return a full single response for the User name and the Post title.
```
query {
    user (id: {userId}) {
    name
   }
    post (id: {postId}) {
    title
   }
}
```

Also, in REST the structure of the data is defined by the resources. GraphQL doesn't require you to structure the data in a particular way. In GraphQL, the structure of the data is defined by the schema. 

GraphQL Schema is a definition of the types of data that are available and how they are related to each other. It is used to validate requests and to generate responses. 

Example:
```
type User {
    id: ID!
    name: String!
}
type Post {
    id: ID!
    title: String!
    author: User!
}
```

In a GraphQL schema, two fundamental components work together to define the structure and behaviour of the API. 
1. Types: define the different types of data that can be returned by the API. For e.g., the User type defines the structure of a User object which might include fields for the user's ID, Name, Email. 
2. Resolvers: 
