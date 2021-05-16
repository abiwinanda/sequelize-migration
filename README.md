# Sequelize Migration Experiment

This is an experiment to understand how to use sequlize migrations.

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

First of all we won't be using anything in the models directory hence feel free to delete this. All created migration will be stored under `migrations` folder and the database config is located under the `config` folder. We will be using `.js` file to read the database config instead of `config.json` file so that we could store the DB config inside environment variables.

To use `.js` file inside 

## Create DB

To start creating the DB you could run the following command

```sh
npx sequelize-cli db:create
```

## Run Migration

Once the migration files has been created, run the following command to run the migration

```sh
npx sequelize-cli db:migrate
```
