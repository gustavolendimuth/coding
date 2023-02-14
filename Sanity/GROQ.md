## Query com Params
```js
const query = '*[_type == "bike" && seats >= $minSeats] {name, seats}'
const params = {minSeats: 2}

client.fetch(query, params).then((bikes) => {
  console.log('Bikes with more than one seat:')
  bikes.forEach((bike) => {
    console.log(`${bike.name} (${bike.seats} seats)`)
  })
})
```

## Querys

```javascript
*[_type == "products" ].categories[]._ref

*[_type == "categories" && _id in *[_type == "products" ].categories[]._ref && isMainCategory]
 
array::unique(*[_type == "products" ].categories[]._ref)

*[_type == "categories" && _id in array::unique(*[_type == "products" ].categories[]._ref) && isMainCategory] {
      name, _id, icon, slug,
      "subCategories": *[_type == "products" && references(^._id)]{
        ...(categories[1]->{name, slug, _id, icon})
      }
}

*[_type == "categories" && isMainCategory] {
      name, _id, icon, slug,
      "subCategories": *[_type == "products" && references(^._id)]{
        ...(categories[1]->{name, slug, _id, icon})
      }
}

{
  "categories": array::unique(*[_type == "products" ].categories[]._ref)
}
{
  "results": *[_type == "categories" && _id in ^.categories && isMainCategory],
}.result
```

https://www.sanity.io/docs/js-client

