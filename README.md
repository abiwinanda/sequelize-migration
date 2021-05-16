# Sequelize Migration Experiment

This is an experiment to understand how to use sequlize migrations.

## Project Set Up

Before you could experiment with this project make sure you have npx installed.

After that make sure you install all required dependencies by running

```sh
npm install
```

Finally make sure you have a running postgres container to experiment with sequelize migration. To create one simply run the following command

```sh
docker-compose up -d
```

## Initializing sequelize migrations

To initialize sequelize migration you could run the following command

```sh
npx sequelize-cli init
```

This will create the following files:
  * `config/config.json`
  * `migrations`
  * `models/index.js`
  * `seeders`

First of all we won't be using anything in the models directory hence feel free to delete this. All created migration will be stored under `migrations` folder and the database config is located under the `config` folder. 

## Set Up DB Config

First of all we will be using `.js` file to read the database config instead of pre-created `config.json` file so that we could store the DB config inside environment variables. 

To make config file with `.js` extension create a `.sequelizerc` file that tell `sequelize-cli` which config file it should use.

```js
// inside .sequelizerc
const path = require('path');

module.exports = {
  // This tells sequelize-cli to use config/config.js file as the DB config
  'config': path.resolve('config', 'config.js'),
  // This tells sequelize-cli that all migrations file are stored under ./migrations/ directory
  'migrations-path': path.resolve('.', 'migrations')
};
```

After that we could start creating `config.js` file under `config` folder as follows

```js
const fs = require('fs');

module.exports = {
    production: {
        username: process.env.DB_USERNAME,
        password: process.env.DB_PASSWORD,
        database: process.env.DB_NAME,
        host: process.env.DB_HOST,
        port: process.env.DB_PORT,
        dialect: 'postgres'
    }
};
```

Finally you could start setting the DB config via environment variables.

## Create DB

To create the DB that you specified in the config you could run the following command

```sh
npx sequelize-cli db:create
```

This will create an empty DB.

## Create Migration

To start adding tables to your DB, create a migration file by running this command

```sh
npx sequelize-cli migration:generate --name <migration-name>

// Example
npx sequelize-cli migration:generate --name create_table_one
```

This command will create the specified migration file name under the `migrations` folder by default.

You could either constructing the table in the migration file using sequelize query interface API or using a raw sql query. The choice is up to you.

Creating table in sequelize migration using sequelize query interface

```js
up: async (queryInterface, Sequelize) => {
  await queryInterface.createTable('table_one', {
    id: {
      type: Sequelize.UUID,
      primaryKey: true
    },
    name: {
      type: Sequelize.STRING
    },
    email: {
      type: Sequelize.STRING
    },
    is_deleted: {
      type: Sequelize.BOOLEAN
    }
  });
},

down: async (queryInterface, Sequelize) => {
  await queryInterface.dropTable('table_one');
}
```

Creating table in sequelize migration using raw sql query

```js
up: async (queryInterface, Sequelize) => {
  return queryInterface.sequelize.query(`
    CREATE TABLE IF NOT EXISTS table_two (
      id uuid PRIMARY KEY,
      name character varying(255),
      email character varying(255),
      is_deleted boolean
    )
  `);
},

down: async (queryInterface, Sequelize) => {
  return queryInterface.sequelize.query("DROP TABLE table_two");
}
```

## Run Migration

Once the migration files has been created, run the following command to create the table

```sh
npx sequelize-cli db:migrate
```
