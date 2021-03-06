# Instalation

<div class="Alert Alert--warning">

This manual is prepared for the developer installation

</div>

## Requirements

| Name                  | Version                           |
|-----------------------|-----------------------------------|
| Language              | Php >= 7.4                        |
| Database              | Posgress >= 10                    |
| Message Broker*       | RabbitMQ >= 3.6.10                |  

*Use for running application in asynchronous mode.

## Quick Start

Clone [backend project repository][repository] to your local directory:
```
git clone git@github.com:ergonode/backend.git
```

<div class="Alert Alert--info">

Application on default starts on **develop** branch

</div>

If want run last stable application version execute:
```
git checkout master
``` 
In .env file you need to configure database connection

Open your terminal in local project, and execute:
```
composer install
``` 
In .env file you need to configure database connection
```
DATABASE_URL=pgsql://db_user:db_password@127.0.0.1:5432/db_name
```
Generate keys for Json Web Token
```
openssl genrsa -out config/jwt/private.pem -aes256 4096
openssl rsa -pubout -in config/jwt/private.pem -out config/jwt/public.pem
```
While executing above commands you would be asked about password. This password needs to be saved then in .env file 
in line `JWT_PASSPHRASE=yourpassword`

In terminal execute command which configure application (available phing commands):
```
bin/phing build
```

To fill database witch basic data execute command:
```
bin/phing database:fixture
```
or for development data use:
```
bin/phing database:fixture:dev
```

Run build in server
```
bin/console s:r
```

<div class="Alert Alert--warning">

To run frontend application see [frontend installation](frontend/installation)

</div>

## Default user

If you fill database with basic data:

| Role       | Login             | Password |
|------------|-------------------|----------|
| Superadmin | test@ergonode.com |123       |

If you fill database with development data:

| Role             | Login                         | Password |
|------------------|-------------------------------|----------|
| Superadmin       | superadmin@ergonode.com       |123       |
| Admin            | admin@ergonode.com            |123       |
| Data Inputer     | data_inputer@ergonode.com     |123       |
| Category Manager | category_manager@ergonode.com |123       |

# .ENV Configuration

Configuration is available in .env file in main directory.

| Variable              | Description                              | Default           | Options                       |
|-----------------------|------------------------------------------|-------------------|-------------------------------|
| APP_ENV               | Type Environment                         | dev               | prod, dev, test               |
| APP_HOST              | Application host                         | ergonode.local    |                               |
| APP_SCHEMA            | Application schema                       | http              | http, https                   |
| APP_URL               | Type Environment                         | http://ergonaut.local |                           |
| DATABASE_URL          | Database connection                      | pgsql://postgres:123@127.0.0.1:5432/ergonode |    |
| JWT_PRIVATE_KEY_PATH  | path to private JWT key file             | config/jwt/private.pem                       |    |
| JWT_PUBLIC_KEY_PATH   | path to public JWT key file              | config/jwt/public.pem                        |    |
| JWT_PASSPHRASE        | JWT Key password                         | 1234                                         |    |
| JWT_TOKEN_TTL         | JWT Token lifetiem in seconds            | 86400                                        |    |
| CORS_ALLOW_ORIGIN     | Cors access address                      | ^http?://localhost:3000\|$                   |    |


# Phing commands

Those command are available only in development mode

| Commands              | Description                                                                                          | Dependencies                                                                 |
|-----------------------|------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|
| `test`                | Full application  testing (clean database, migrations, fixtures, PHPUnit, Behat)                     | database:migrate, database:fixture, test:unit, test:behat                    |
| `test:unit`           | Unit test - phpunit                                                                                  |                                                                              |
| `test:unit:coverage`  | Unit test - phpunit  with coverage                                                                   |                                                                              | 
| `test:behat`          | Behat - api integration tests                                                                        |                                                                              |
| `database:migrate`    | Run migration to database                                                                            |                                                                              |
| `database:create`     | Create database                                                                                      |                                                                              |
| `database:drop`       | Drop database                                                                                        |                                                                              |         
| `database:fixture`    | Adding basic fixtures to database                                                                    |                                                                              |
| `database:fixture:dev`| Adding development fixtures to database                                                              |                                                                              |
| `check:style`         | Code inspection (CS, MD, CPD)                                                                        | check:php, check:cs, check:cpd, check:md                                     |
| `check:md`            | Mass detector                                                                                        |                                                                              |
| `check:cs`            | Checking coding standards - code sniffer ([coding standards](backend/quality_and_code_standards.md)) |                                                                              |
| `check:cpd`           | Copy/paste detector                                                                                  |                                                                              |
| `check:phpstan`       | PHP stan -level 1 inspection                                                                         |                                                                              |
| `cache:clear`         | Clear cache                                                                                          | cache:clear, create:directory, database:migrate                              |

[repository]: https://github.com/ergonode/backend
