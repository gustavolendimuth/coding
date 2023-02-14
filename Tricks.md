# Geral
## Console.log no meio de uma arrow function

If you want to get a better sense of what's going on, you can log out the `listItem` in the filter like this:

```javascript 
.filter(listItem => console.log(listItem.getId()) || !['siteSettings'].includes(listItem.getId()))
```

Since `console.log()` returns `null` the `or` operator (`||`) will always return the right hand expression.

## Confirmação de valor antes de uma HOF

```javascript
ingredientsData[recipeId]?.some((ingredient) => ingredient === ingredientName(index))
```

## Regex

http://regex101.com
http://regexone.com

# React

## Testing Playground

```javascript 
console.log(screen.logTestingPlaygroundURL());
```
