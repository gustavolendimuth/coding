## Import javascript file

React allows us to build [custom Hooks](https://reactjs.org/docs/hooks-custom.html), which let us extract component logic into reusable functions. The logic to append an external JS file to a component could be stored as a custom Hook as below:

Which could be used in components as below:
```javascript
import importScript from 'customHooks/importScript';const Demo = props => {  
  importScript("/path/to/resource.js");  
}
```

https://betterprogramming.pub/4-ways-of-adding-external-js-files-in-reactjs-823f85de3668