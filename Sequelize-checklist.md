# Sequelize Checklist

## Banco de dados rodando

- [ ] Instancia MySQL rodando.

## Como subir uma instancia MySQL com Docker Compose

Crie na raiz do seu projeto um ``` docker-compose-yml ``` e coloque o seguinte trecho de código:

```
version: '3.1'
services:
  mysql:
    image: mysql:8.0.23
    container_name: container_mysql
    platform: linux/x86_64
    environment:
      - MYSQL_ROOT_PASSWORD=password
    ports:
      - 33065:3306
  api:
    container_name: api
    image: node:16
    restart: always
    ports:
      - 3001:3001
    environment:
      - DB_HOST=mysql
      - DB_NAME=
      - DB_USER=root
      - DB_PASSWORD=password
      - SERVER_PORT=3001
    volumes:
      - ./:/usr/app
    working_dir: /usr/app
    command: bash
    # As duas opções abaixo correspondem ao -it
    tty: true # -t
    stdin_open: true #-i 
    depends_on:
      - mysql
```

Rode o comando `docker-compose up -d`.

## Instalação das dependências

**OBS: Na raiz da pasta do projeto**

- [ ] ``` npm init -y ```
- [ ] ``` npm install express express-async-errors sequelize mysql2 dotenv ```
- [ ] ``` npm -D install nodemon sequelize-cli ```
- [ ] Criar uma pasta ``` src ```
- [ ] Criar o arquivo ``` .sequelizerc ```  na raiz do projeto com o seguinte código:

**Código do ``` .sequelizerc ```**

```javascript
const path = require('path');

module.exports = {
  'config': path.resolve('src', 'config', 'config.js'),
  'models-path': path.resolve('src', 'models'),
  'seeders-path': path.resolve('src', 'seeders'),
  'migrations-path': path.resolve('src', 'migrations'),
};
```

- [ ] Acesse a pasta ``` src ``` e rode o seguinte comando: ``` npx sequelize-cli init ```
- [ ] Verifique se as pastas config, models, migrations e seeders foram criadas
- [ ] Sair da pasta ``` src ```
- [ ] Alterar a extensão do arquivo ``` config.json ``` para config.js e colocar o seguinte trecho de código

**Código do ``` config.js ```**

```javascript
require('dotenv').config();

const config = {
  username: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME,
  host: process.env.DB_HOST,
  dialect: process.env.DB_DIALECT,
  port: process.env.DB_PORT,

};

module.exports = {
  development: config,
  test: config,
  production: config,
};
```

- [ ] Altere a linha 9 do arquivo ``` /models/index.js ```

``` Javascript
// configuração antiga
const config = require(__dirname + '/../config/config.json')[env];

// configuração nova
const config = require(__dirname + '/../config/config.js')[env];
```

## Conexão com o banco

- [ ] Crie o arquivo .env na raiz do projeto
- [ ] Colocar o seguinte trecho de código no ``` .env ```

**Código do ``` .env ```**

```
DB_USER=root
DB_PASSWORD=password
DB_NAME=trybe_orm
DB_HOST=localhost
DB_DIALECT=mysql
DB_PORT=33065
```

- [x] rodar o comando ``` npx sequelize db:create ```
- [ ] rodar o comando ``` npx sequelize model:generate --name course --underscored --attributes name:string,description:string,creation_date:date,active:boolean ```
- [ ] Dentro da pasta model, crie um arquivo course.js e coloque o seguinte código:

**Código da model course.js**

```javascript
const { DataTypes } = require("sequelize");

const CourseSchema = (sequelize, DataTypes) => {
 const CourseTable = sequelize.define('Course', {
   name: DataTypes.STRING,
   description: DataTypes.STRING,
   creation_date: DataTypes.DATE,
   active: DataTypes.BOOLEAN
 }, 
 {
   tableName: 'courses',
   underscored: true
 })
 return CourseTable;
}

module.exports = CourseSchema;
```

- [ ] rodar ``` npx sequelize db:migrate ```
- [ ] rodar ``` npx sequelize seed:generate --name course ```
- [ ] Popular a seed exemplo no [código da aula 6.1](https://github.com/tryber/sd-022-b-live-lectures/blob/lecture/24.1/projeto/src/seeders/20221019210353-course.js) e remover o atributo duration dos objetos neste arquivo ou criar uma coluna duration do tipo inteiro
- [ ] rodar ``` npx sequelize db:seed:all ``` 

## Funcionalidades do Projeto

- [ ] Agora estamos com nossa tabela courses criada e populada no banco, a model course também já foi criada
- [ ] A partir deste momento, você deve seguir os mesmos passos que viu nas seções anteriores para a criação do service e controller para course, além da criação das rotas, middlewares e os arquivos ``` app.js ``` e ``` server.js ```
- [ ] Lembre também que os métodos chamados no service são nativos da biblioteca do ``` Sequelize ```, como o create para inserir registros, o ``` findAll ``` para buscar todos registros, o update para atualizar um registro e o destroy para remover um registro