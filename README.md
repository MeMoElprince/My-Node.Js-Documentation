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

### Query Sorting
> Sorting the query by some fields

```js
let query = Tour.find(queryObj);
if (req.query.sort) {
    // we provide query like this ?sort=price,ratingsAverage,ratingsQuantity
    // so we need to replace each ',' with a space
    // to make it like this 'price ratingsAverage ratingsQuantity'
  const sortBy = req.query.sort.split(',').join(' ');
  query = query.sort(sortBy);
} else {
  query = query.sort('-createdAt');
//   default sorting when no query is provided
}
```
* <b>req.query.sort.split(',').join(' ')</b> used for replacing the comma with space
* <b>query = query.sort(sortBy)</b> used for sorting the query by the fields provided in the query
* <b>query = query.sort('-createdAt')</b> used for default sorting when no query is provided
* <b>sort method</b> is used for sorting the query by the fields provided in the query in order of the fields provided


<hr>
