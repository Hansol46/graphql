# about GraphQL

**GraphQL** is a query language created by Facebook in 2012 due to collisions with the limitations of traditional **REST APIs** (more on that later). Since **GraphQL** is just a specification, the server can be implemented in Node.js, php, Ruby; and the client side, for example, on Relay or Apollo (there are both React-Apollo and Angular-Apollo). To compare **GraphQL** and **REST**, you can use a metaphor. Traditional **REST** is like making an appointment with a dentist, ordering delivery, and calling a car repair service using three phone calls to three different phone numbers.

<p align="center" display="flex">
  <img src="https://i.ibb.co/jkQZ6KP/image.png" width="400" height="400" title="hover text">
</p>

**GraphQL**, in turn, acts as our personal assistant, allowing us to give him the opportunity to book you to the dentist, deliver delivery and connect with the machine repair service. We ask for what we need and then just wait.

<p align="center" display="flex">
  <img src="https://i.ibb.co/P5BsW0w/image.png" width="400" height="400" title="hover text">
</p>

<hr />

## Where to begin?

Creating a schema. The schema is the skeleton of the entire server side, which contains all our requests. We can consider an example of a schema:

<p align="center" display="flex">
  <img src="https://i.ibb.co/3mTcSQG/image.png" width="400" height="400" title="hover text">
</p>
This schema describes what entities we have in the application and what manipulations we can generally perform with them. Let's talk in more detail about entities and queries.

## 1. Query queries in GraphQL

Using queries, **GraphQL** retrieves the necessary data from the server. **Query** in **GraphQL** is analogous to **GET** in **REST**. Query describes the data to be retrieved from the server. For example, our scheme, using the code below, you can get an array of books `Book` and the second request is to get a book by `id`.

```
        type Query {
	        getAllBooks: [Book]!
	        getBook(id: ID!): Book!
        }
```

Response to this data comes in **JSON** format. Successful operations return a **JSON** with the key and the data, and unsuccessful return **JSON** with the key and the error message. This makes it convenient to handle errors on the client side.

## 2. Mutation в GraphQL

**Mutation** is another root type. If **Query** is analogous to **GET**, then **Mutation** is analogous to **POST** and **PUT** in **REST**. Using this method, you can mutate or, more simply, change the data, for example, add some data to the database. Example code for adding a new book:

```
        type Mutation {
	        addBook(book: BookInput!): Boolean!
        }
```

This create mutation `addBoook`, which adds a new book to the database. 

## 3. Subscription в GraphQL

**Subscription** is the third type of operation in **GraphQL**. With its help, the client listens to changes in the database in real time. Under the hood, subscriptions use websockets. Example code (relatively speaking, if it was possible to add like books):

```
        subscription listenLikes {
          listenLikes {
            Book
            likes
          }
        }

```

With this query, you can get a list of users with names and number of likes every time a user gets a like.

<hr />

## Next step - create resolvers 

To bind our data to the schema, we use **resolvers**, in our case it is `root`. The fields in this object fully correspond to what was in the schema, all we need to do is specify which function to execute when a field is requested

<p align="center" display="flex">
  <img src="https://i.ibb.co/jGMvQQW/image.png" width="400" height="400" title="hover text">
</p>

In our case, `allBookis` just an array of books containing books in the required format

## Convenient console 

Included with GraphQL comes GraphiQL - a special console that visually displays requests. View example:
<p align="center" display="flex">
  <img src="https://i.ibb.co/KX3k8dz/image.png" width="600" height="400" title="hover text">
</p>

In it, we can write the queries we need and immediately see the result. What is even more convenient, this console is documentation, on the right on thebutton **Docs** all entities, mutations that are in our schema are described

## Display data 

Great way is to use Apollo https://www.apollographql.com

Let's analyze in order what needs to be done for this.

* To get started we need to install Apollo ``` npm install @apollo/client graphql ```

* Next we need to create `client`

```
		const client = new ApolloClient({
		  uri: 'https://48p1r2roz4.sse.codesandbox.io',
		  cache: new InMemoryCache()
		});
```

* Next step, wrap our  `<App />`

```
		function App() {
		  return (
		    <ApolloProvider client={client}>
		      <div>
			<h2>My first Apollo app 🚀</h2>
		      </div>
		    </ApolloProvider>
		  );
		}
```

* Remains to make a request, in Apollo a special function helps us with this `gql`

```
		const EXCHANGE_RATES = gql`
		  query GetExchangeRates {
		    rates(currency: "USD") {
		      currency
		      rate
		    }
		  }
		`;
```

* Finally, display the data by placing our variable in the hook `useQuert`

```
		function ExchangeRates() {
		  const { loading, error, data } = useQuery(EXCHANGE_RATES);

		  if (loading) return <p>Loading...</p>;
		  if (error) return <p>Error :(</p>;

		  return data.rates.map(({ currency, rate }) => (
		    <div key={currency}>
		      <p>
			{currency}: {rate}
		      </p>
		    </div>
		  ));
		}
```

<hr />


# Positive and negative GraphQL

Let's features and disadvantages of GraphQL.

## Positive: 

   1. **No overabundance when working with GraphQL**
When working with GraphQL, there is no superfluous information. We only get what we ask for.
GraphQL minimizes the amount of data transferred over the network by selectively picking it up and guided primarily by the needs of the client application.

   2. **Strongly Typed GraphQL**
GraphQL is a strongly typed query language written in an Expressive Schema Definition Language (SDL) for GraphQL. This language has the same benefits as any strongly typed programming language. Obviously this approach is less error prone

   3. **One Source of Truth**
There is one truth in GraphQL applications and that is the GraphQL schema. It is she who is the central source, which describes all the available data. clients can read (query) and write (modify) data based on this schema

   4. **Versions of GraphQL**
GraphQL doesn't have the same API versions that we're used to in REST. In REST, it's okay to offer multiple versions of the same API (eg api.domain.com/v1/, api.domain.com/v2/) because resources or their structure can change over time. In GraphQL, you can translate APIs into deprecated fields at the field level. Consequently, the client receives a warning when they access a deprecated field. After some time, the deprecated field can be excluded from the schema, then no more clients will use it. This way, the GraphQL API can evolve without the need for versioning

## Negative 

   1. **Problem N + 1**
GraphQL is built on resolvers functions. This means that fetching data from the database can cause a problem called SELECT N + 1. Suppose that a list of objects was received in the resolver, in which the data associated with this object is represented by identifiers (foreign keys). For each such identifier, its own function-resolver will be called, in which (in each) a query to the database will be additionally made. Thus, instead of one query to the database, many queries will be executed, which overloads the database with queries.
To solve this problem, facebook has developed a library github.com/graphql/dataloaderwhich uses the deferred query strategy
   2. **Rate limiting in GraphQL**
Another issue is rate limiting. While in REST it is relatively easy to say “no more than that many requests per day”, it is difficult to formulate such a statement for individual GraphQL operations, since there are not only “expensive” and “not expensive” operations, but many intermediate gradations. It is for such cases that companies Providing public GraphQL APIs, they offer their own rate limiting calculations, often boiling down to the aforementioned maximum query depths and weighting of query complexity.
   3. **Caching GraphQL**
When working with GraphQL, implementing a lightweight cache is much more complex than implementing REST. Working with REST, we refer to resources by URL and, therefore, we can organize caching at the resource level, since the URL of a resource can serve as its identifier. In GraphQL, this gets complicated because all requests can be different, even though everyone is operating on the same object. In one request, you can ask for the author's name, and in the next, not only the author's name, but also his email address. This is where you need a more subtle field-level cache, and it's not easy to implement. However, most libraries built on top of GraphQL offer such caching mechanisms out of the box.

***And most importantly, GraphQL is not a database***




