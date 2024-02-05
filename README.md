# Node.JS
<hr>


## Mongoose Tips

#### <i>Query filtering</i>
> Removing some keywords from the query

```js
let queryObj = {...req.query};
const queryExclude = ['page', 'sort', 'limit', 'fields'];
queryExclude.forEach(data => delete queryObj[data]);
```


#### <i>Advanced query filtering</i>
> Using regex replacing some keywords with another words

```js
let queryStr = JSON.stringify(queryObj);
queryStr = queryStr.replace(/\b(gte|gt|lte|lt)\b/g, match => `$${match}`);
queryObj = JSON.parse(queryStr);
```
* <b>\b</b> used for matching exactly that is inside 
* <b>g</b> used for every matching will change not just the first one

<hr>
