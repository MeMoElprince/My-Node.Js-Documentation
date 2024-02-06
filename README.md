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

#### <i>Query Sorting</i>
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

#### <i> Query Fields Limiting </i>
> Limiting the fields to be shown in the query

```js
let query = Tour.find(queryObj);
if (req.query.fields) {
  const fields = req.query.fields.split(',').join(' ');
  query = query.select(fields);
} else {
  query = query.select('-__v');
}
```
* <b>req.query.fields.split(',').join(' ')</b> used for replacing the comma with space
* <b>query = query.select(fields)</b> used for selecting the fields provided in the query
* <b>query = query.select('-__v')</b> used for default selecting when no query is provided

#### <i> Query Pagination </i>
> Paginating the query

```js
let query = Tour.find(queryObj);
const page = req.query.page * 1 || 1;
const limit = req.query.limit * 1 || 100;
const skip = (page - 1) * limit;
query = query.skip(skip).limit(limit);

if (req.query.page) {
  const numTours = await Tour.countDocuments();
  if (skip >= numTours) throw new Error('This page does not exist');
}
```
* <b>req.query.page * 1 || 1</b> used for converting the query to number and if it is not provided it will be 1
* <b>req.query.limit * 1 || 100</b> used for converting the query to number and if it is not provided it will be 100
* <b>const skip = (page - 1) * limit</b> used for skipping the number of documents
* <b>query = query.skip(skip).limit(limit)</b> used for skipping the number of documents and limiting the number of documents
* <b>const numTours = await Tour.countDocuments()</b> used for counting the number of documents
<hr>
