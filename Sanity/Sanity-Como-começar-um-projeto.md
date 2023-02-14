# Projeto Sanity

Para começar um projeto do Sanity, primeiro criar um projeto do React, depois fazer um push para o Github.

Seguir os passos:

1. Instalar o Sanity CLI globalmente, caso ainda não esteja instalado ``` npm i -g @sanity/cli ```.
2. Criar um projeto Sanity dentro da pasta do projeto React ``` sanity init ```.
3. Para iniciar o projeto usar o comando ``` sanity start ``` dentro da pasta do projeto Sanity.
6. Para conectar os dois projetos executar na pasta do projeto React ``` npm install @sanity/client ```.
7. Criar arquivo client.js na pasta src do React com o código:

```javascript
import sanityClient from "@sanity/client";

export default sanityClient({
	projectId: "Your Project ID Here", // find this at manage.sanity.io or in your sanity.json
	dataset: "production", // this is from those question during 'sanity init'
	useCdn: true,
});
```

6. Na página de configuração do projeto no sanity.io, na aba de API, adicionar novo CORS Origin o localhost:3000.


## Deploy

1. Adicionar caminho do servidor

```javascript
"project": {
	"name": "Nome do Projeto",
	"basePath": "/studio"
},
```

2. Instalar o sanity CLI ou adicionar no package.json.
3. Adicionar comando para build no package.json do projeto React.

```javascript
"scripts": {
	"build": "react-scripts build && cd sanity && sanity install && sanity build ../build/studio -y",
}
```

## Referência
https://www.sanity.io/guides/build-your-first-blog-using-react
https://www.sanity.io/docs/deployment