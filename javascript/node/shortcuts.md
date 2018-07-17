## To generate a new MVC app

1. `express <app_name>`
2. `cd <app_name>`
3. `sequelize init`

## To start app

`DEBUG=tryexpress:* npm start`

## User Authentication

 1. `npm install bcrypt bcrypt-promise`
 2. `npm install csurf cookie-session`

## Cookie and CSRF
```
const cookie = require("cookie-session");
const csrf = require("csurf");

app.use(
  cookieSession({
    name: "helloworld",
    keys: ["fwfwfewfew", "hgowonbfoeg"],
    maxAge: 1000 * 60 * 60 * 24 // 24 hours;
  })
);

app.use(
  csrf({
    cookie: true
  })
);
```

## Favicon
```
app.use(favicon(path.join(__dirname, "public")));
```

## Using Postgres with Sequelize

Install dependencies
```
$ npm install sequelize pg pg-hstore
$ npm install sequelize-cli
## OR TO HAVE SEQUELIZE COMMAND ##
$ npm install -g sequelize-cli
```

Create skeleton for application
```
$ node_modules/.bin/sequelize init
```

To deploy to Heroku
```
// renamed from config/config.json to config/database.js

if (process.env.NODE_ENV == "production") {
  const databaseUrl = process.env.DATABASE_URL;
  const herokuPostgresRegex = /postgres:\/\/([\S]+):([\S]+)@([\S]+):5432\/([\S]+)/y;
  var databaseCredentials = herokuPostgresRegex.exec(databaseUrl);
}

module.exports = {
  development: {
    database: "bookkeep_development",
    host: "127.0.0.1",
    dialect: "postgres"
  },
  test: {
    database: "bookkeep_test",
    host: "127.0.0.1",
    dialect: "postgres"
  },
  production: {
    username: databaseCredentials[1],
    password: databaseCredentials[2],
    database: databaseCredentials[4],
    host: databaseCredentials[3],
    dialect: "postgres"
  }
};
```

```
//.sequelizerc (create this file in application root directory)
const path = require('path');

module.exports = {
  'config': path.resolve('config', 'database.js')
}
```

In `models/index.js`, modify
`var config = require(__dirname + "/../config/config.js")[env];`
to
`const config = require(__dirname + "/../config/database.js")[env];`


Create Database

```
$ node_modules/.bin/sequelize db:create
```

Create Model and Migration
```
$ node_modules/.bin/sequelize model:create --name User --attributes firstName:string,lastName:string,email:string
```

Create Migration
```
$ node_modules/.bin/sequelize migration:create --name User --attributes firstName:string,lastName:string,email:string

```

Run Migrations
```
$ node_modules/.bin/sequelize db:migrate
```

Undo Migrations
```
$ node_modules/.bin/sequelize db:migrate:undo
$ node_modules/.bin/sequelize db:migrate:undo:all
$ node_modules/.bin/sequelize db:migrate:undo:all --to XXXXXXXXXXXXXX-create-posts.js
```

Creating Seed
```
$ node_modules/.bin/sequelize seed:generate --name demo-user
```

```
// seeders/XXXXXXXXXXXXXX-demo-user.js

'use strict';

module.exports = {
  up: (queryInterface, Sequelize) => {
    return queryInterface.bulkInsert('Users', [{
        firstName: 'John',
        lastName: 'Doe',
        email: 'demo@demo.com'
      }], {});
  },

  down: (queryInterface, Sequelize) => {
    return queryInterface.bulkDelete('Users', null, {});
  }
};
```

Running Seed
```
$ node_modules/.bin/sequelize db:seed:all
```

Undoing Seed
```
$ node_modules/.bin/sequelize db:seed:undo
$ node_modules/.bin/sequelize db:seed:undo:all
```