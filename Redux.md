# Checklist do react-redux

### _Antes de começar_
- [ ] pensar como será o _formato_ do seu estado global
- [ ] pensar quais actions serão necessárias na sua aplicação

### _Instalação_

 - [ ] **Executar comando de instalação dentro do diretório do projeto**

```zsh
npm i redux react-redux;
npm i redux-devtools-extension;
```

##### _Criar dentro do diretório src:_
- [ ] diretório `redux`

##### _Criar dentro do diretório `redux`_
- [ ] diretório `store`
- [ ] diretório `actions`
- [ ] diretório `reducers`

##### _Criar dentro do diretório `actions`:_
- [ ] arquivo `index.js`.

##### _Criar dentro do diretório `reducers`:_
- [ ] arquivo `index.js`.

##### _Criar dentro do diretório `store`:_
- [ ] arquivo `index.js`.

##### _Criar dentro do diretório `reducers`:_
- [ ] criar os reducers necessários
- [ ] criar `rootReducer` usando o `combineReducers` no arquivo index.js

**exemplo:**

_Seu reducer pode seguir esse modelo._

```javascript
const INITIAL_STATE = {};

const nomeReducer1 = (state = INITIAL_STATE, action) => {
 switch(action.type) {
   default:
    return state;
 }
}

export default nomeReducer1;
```

_O rootReducer que é a combinação de todos os reducers_

```javascript
import { combineReducers } from 'redux';
import nomeReducer1 from './nomeReducer1';
import nomeReducer2 from './nomeReducer2';

const rootReducer = combineReducers({
  nomeReducer1,
  nomeReducer2,
});

export default rootReducer;
```

### _No arquivo store/index.js:_
- [ ] importar `rootReducer` e usá-lo na criação da `store`
- [ ] configurar o [Redux DevTools](https://github.com/reduxjs/redux-devtools)
- [ ] exportar a `store`

```javascript
import { createStore } from 'redux';
import { composeWithDevTools } from 'redux-devtools-extension';
import rootReducer from '../reducers';

const store = createStore(
  rootReducer,
  composeWithDevTools(),
);

export default store;
```

### _No arquivo App.js:_
- [ ] importar a `store`
- [ ] definir o Provider, `<Provider store={ store }>`, para fornecer os estados a todos os componentes encapsulados em `<App />`.

### _Na pasta actions:_
- [ ] criar e exportar os actionTypes;`
- [ ] criar e exportar os actions creators necessários

**_Exemplo de action types (arquivo actionTypes.js)_**

```javascript
export const USER_LOGIN = 'USER_LOGIN';
```

**_Exemplo action creator_**

```javascript
import { USER_LOGIN } from '../actions/actionTypes';
export const minhaAction = (value) => ({ type: USER_LOGIN, value });
```

### _Nos reducers:_
- [ ] criar os casos para cada action criada, retornando o devido estado atualizado

### _Nos componentes que irão ler o estado:_

- [ ] criar a função `mapStateToProps`
- [ ] exportar usando o `connect`

### _Nos componentes que irão modificar o estado:_

- [ ] criar a função `mapDispatchToProps`
- [ ] exportar usando o `connect`

```javascript
export default connect(mapStateToProps, mapDispatchToProps)(Component)
```

## Entendendo o infograma de uma store

![Infograma de store](./images/store.png)


### Análise do passo a passo

![Store](./images/store.png)

## Reducer

**Exemplo /src/reducers/myReducer.js:**

```jsx
const INITIAL_STATE = {
  state: '',
};

function myReducer(state = INITIAL_STATE, action) {
  switch (action.type) {
    case 'NEW_ACTION':
      return { state: action.state };
    default:
      return state;
  }
}

export default myReducer;
```

##### Root Reducer
**/src/reducers/index.js

```jsx
import { combineReducers } from 'redux';
import myReducer from './myReducer';

const rootReducer = combineReducers({ myReducer });

export default rootReducer;
```

## Store

**Exemplo /src/store/index.js: 

```jsx
import { createStore, combineReducers } from 'redux';
import rootReducer from '../reducers';

const store = createStore(rootReducer);

export default store;
```

## Actions

**Exemplo /src/actions/index.js**

```jsx
export const newAction = (state) => ({ type: 'NEW_ACTION', state });
```

## Diferentes formas de usar o dispatch
- https://react-redux.js.org/using-react-redux/connect-mapdispatch
