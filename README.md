# about GraphQL


GraphQL – это язык запросов, созданный в компании Facebook в 2012 году, причиной этому стали столкновения с ограничениями традиционных REST API интерфейсов (поговорим об этом позже). Так как GraphQL, это просто спецификация, то сервер можно реализовывать на Node.js, php, Ruby; а клиентскую часть, к примеру на Relay или Apollo (есть как React-Apollo, так и Angular-Apollo).
Для сравнения GraphQL и REST, можно использовать метафору. Традиционный REST это, как если бы мы записывались к стоматологу, заказывали доставку, и звонили в сервис ремонта машин используя три телефонных звонка, на три разных телефонных номера. 

<p align="center" display="flex">
  <img src="https://i.ibb.co/jkQZ6KP/image.png" width="400" height="400" title="hover text">
</p>

GraphQL в свою очередь выступаем как наш личный помощник, позволяя нам передать ему записать вас к стоматологу, привезти доставку и связать с сервисом ремонта машин. Мы запрашиваем то, что нам необходимо и затем просто ждем.

<p align="center" display="flex">
  <img src="https://i.ibb.co/P5BsW0w/image.png" width="400" height="400" title="hover text">
</p>

<hr />
### Where to begin?

Создание схемы
Схема это скелет всей серверной части, можем рассмотреть пример схемы:

<p align="center" display="flex">
  <img src="https://i.ibb.co/fdZ3fkL/img-site2.png" width="400" height="400" title="hover text">
</p>
Это схема описывает какие сущности есть у нас в приложении и какие манипуляции мы вообще можем с ними производить. Поговорим более подробно об этих сущностях.


## 1. Query запросы в GraphQL
С помощью запросов GraphQL получает необходимые данные с сервера. Query в GraphQL — это аналог GET в REST. Query описывает данные, которые необходимо получить с сервера. Например, с помощью кода ниже можно получить name и age всех пользователей в базе данных. 

// пример кода

Ответ на эти данные приходит в формате JSON. Успешные операции возвращают JSON с ключом и данными, а неуспешные возвращают JSON с ключом и сообщением об ошибке. Благодаря этому удобно обрабатывать ошибки на стороне клиента.


## 2. Mutation в GraphQL
Mutation — ещё один корневой тип. Если Query это аналог GET, то Mutation - аналог POST и PUT в REST. С помощью этого метода, можно мутировать или проще говоря изменять данные, к примеру добавлять какие-нибудь данные в базу данных Ниже пример кода.

// пример кода

Здесь создаётся мутация createUser, которая добавляет в БД пользователя с name Richie и age 22. В ответ на этот запрос сервер присылает JSON с id записи.


## 3. Subscription в GraphQL
Subscription — третий тип операций в GraphQL. С его помощью клиент слушает изменения в БД в режиме реального времени. Под капотом подписки используют вебсокеты. Пример кода:

// пример кода

С помощью этого запроса можно получать список пользователей с именами и количеством лайков каждый раз, когда пользователь получает лайк.

<hr />
### Next step - create resolvers 

Чтобы привязать наши данные к схеме, мы используем resolvers, в нашем случае это `root`. Поля в этом объекте полностью соответствуют тому, что был в схеме, всё что нам нужно сделать это указать какая функция выполниться при запросе какого-либо поля
<p align="center" display="flex">
  <img src="https://i.ibb.co/fdZ3fkL/img-site2.png" width="400" height="400" title="hover text">
</p>
В нашем случае `allBooks` это просто массив книг, в котором лежат книги в нужном формате
