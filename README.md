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
Subscription — третий тип операций в GraphQL. С его помощью клиент слушает изменения в БД в режиме реального времени. Под капотом подписки используют вебсокеты. Пример кода(условно говоря если бы можно было ставить лайки книгам):

```
        subscription listenLikes {
          listenLikes {
            Book
            likes
          }
        }

```

С помощью этого запроса можно получать список пользователей с именами и количеством лайков каждый раз, когда пользователь получает лайк.

<hr />


## Next step - create resolvers 

Чтобы привязать наши данные к схеме, мы используем **resolvers**, в нашем случае это `root`. Поля в этом объекте полностью соответствуют тому, что был в схеме, всё что нам нужно сделать это указать какая функция выполниться при запросе какого-либо поля
<p align="center" display="flex">
  <img src="https://i.ibb.co/jGMvQQW/image.png" width="400" height="400" title="hover text">
</p>

В нашем случае `allBook`, это просто массив книг, в котором лежат книги в нужном формате

## Удобная консоль 

В комплекте с **GraphQL** идет **GraphiQL** - специльная консоль, которая визуально отображает запросы. Пример отображения 
<p align="center" display="flex">
  <img src="https://i.ibb.co/KX3k8dz/image.png" width="600" height="400" title="hover text">
</p>

В ней мы можем писать необходимые нам запросы и сразу видеть результат. Что еще более удобно эта консоль является документацией, справа на кнопке **Docs** описаны все сущности, мутации, которые есть в нашей схеме.

## Отображение данных 

Отличным способом является использование Apollo <https://www.apollographql.com>

Разберем по порядку что для этого необходимо сделать.

* Чтобы начать нам необходимо установить Apollo ``` npm install @apollo/client graphql ```

* Далее необходимо создать `client`

```
		const client = new ApolloClient({
		  uri: 'https://48p1r2roz4.sse.codesandbox.io',
		  cache: new InMemoryCache()
		});
```

* Следующий шаг, оборачиваем наш `<App />`

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

* Осталось сделать запрос, в Apollo нам в этом помогает специальная функция `gql`

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

* И наконец отобразить данные, помещая нашу переменную в hook `useQuert`

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

Разберем особенности и недостатки GraphQL. Начнем 
