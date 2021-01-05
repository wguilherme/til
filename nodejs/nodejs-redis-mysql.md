# Today I learned to work with Redis in a NodeJS Application


Today I learned how to use NodeJS, MySQL and Redis together to cache data and optimize database requests. This simple project, we make a request for 1000 user records and store the cache. We make the requisition 1000 times. With cached data with redis, we noticed a reduction of approximately 50% to perform the same process directly into database.


## Study
For this project, we'll use `promise-redis` instead `redis` package for use async/await.

```
yarn add promise-redis
```

Create a index.js file

```js
//index.js
const redis = rquire('promise-redis')(); 
const client = redis.createClient();
client.set('key', 'value')
```
The `client.set();` stores the value in the specified key. You can access this value, later, with `client.get('key');

Lets put all in asyncronous function for use async/await in redis.
```js
//index.js
(async()=> {
const redis = require('promise-redis')();
const client = redis.createClient();
const result = await client.set('key', 'value');
})();
```

For this study and tests I used an redis image from docker. First we need to pull the redis image. With the docker installed and running, run this command in your terminal:
```docker pull redis```

Now, lets create a redis container and expose the default port to our system. Run this command in your terminal:

```docker run -p 127.0.0.1:6379:6379 --name local-redis -d redis````

Now, Redis is running at 6379 port. Lets test creating a data in redis and then, log the data. This should returns the 'value' string.

```js
//index.js
(async()=> {
const redis = require('promise-redis')();
const client = redis.createClient();
const result = await client.set('key', 'value');
const getResult = await client.get('key');
console.log(getResult);
})();
```

The next step, we'll define a expire time in seconds using two parameters in redis `client.set()`. First, after key and value we need to pass the 'EX' string, then the time in seconds. Like this:

```js
client.set('key', 'value', 'EX', 10);
```

Now, lets use redis to cache data from a real database  request. In this example we'll use mysql database and muysql2 package to connect and get data. Lets use a mysql image from docker. Run in your terminal:
```docker pull mysql````

Now, lets create a mysql container and expose the default port to our system. Run this command in your terminal:

```docker run -p 3306:3306 --name local-mysql -e MYSQL_ROOT_PASSWORD=root -d mysql:8.0.16```

To see the redis/cache performance in action, lets use a 1.000 records data. For this, I used the online tool http://filldb.info/dummy/step1 to generate fake user data. The schema has id, name and age fields.

See the complete implementation to get data from mysql and, if have a cached data, show cached user instead of  make a new connection/request in database.

```js

(async () => {
   const redis = require('promise-redis')();
   const client = redis.createClient();
   const mysql = require('mysql2/promise');
   const conn = await mysql.createConnection("mysql://root:root@localhost:3306/users");

   // cache client
   const clientCache = await client.set('800', 'Victoria 7073 (cached)', 'EX', 20);

   const userId = 800;

   console.time('mysql')
   for (let x = 0; x < 1000; x++) {
      let cachedUser = await client.get(userId);

      if (!cachedUser) {
         const [rows] = await conn.query('select * from users where id=? limit 1', [800]);
         console.log(rows[0].name, rows[0].age);
      } else {
         console.log('aqui', cachedUser);
      }
   }
   console.timeEnd('mysql');

})();
```




Running the above code and reversing the '!' in the if, you can compare the performance between showing the cached version and executing the 1,000 connection calls and searching in database. In this sample of 1,000 calls, we can see a performance gain of approximately 50%. The greater the number of requests, the greater the performance loss compared to showing the cached version.