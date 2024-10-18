# MongoDB

## Create Collection
There are 2 ways to create a collection.

Method 1
You can create a collection using the createCollection() database method.

```sh
db.createCollection("posts")
```

Method 2
You can also create a collection during the insert process. We are here assuming object is a valid JavaScript object containing post data:

```sh
db.posts.insertOne(object)
```

This will create the "posts" collection if it does not already exist.

## Insert Documents
There are 2 methods to insert documents into a MongoDB database.

insertOne()
To insert a single document, use the insertOne() method.

This method inserts a single object into the database.

Note: When typing in the shell, after opening an object with curly braces "{" you can press enter to start a new line in the editor without executing the command. The command will execute when you press enter after closing the braces.

```sh
db.posts.insertOne({
  title: "Post Title 1",
  body: "Body of post.",
  category: "News",
  likes: 1,
  tags: ["news", "events"],
  date: Date()
})
```
Note: If you try to insert documents into a collection that does not exist, MongoDB will create the collection automatically.

insertMany()
To insert multiple documents at once, use the insertMany() method.

This method inserts an array of objects into the database.

```sh
db.posts.insertMany([  
  {
    title: "Post Title 2",
    body: "Body of post.",
    category: "Event",
    likes: 2,
    tags: ["news", "events"],
    date: Date()
  },
  {
    title: "Post Title 3",
    body: "Body of post.",
    category: "Technology",
    likes: 3,
    tags: ["news", "events"],
    date: Date()
  },
  {
    title: "Post Title 4",
    body: "Body of post.",
    category: "Event",
    likes: 4,
    tags: ["news", "events"],
    date: Date()
  }
])
```

## Find Data
There are 2 methods to find and select data from a MongoDB collection, find() and findOne().

find()
To select data from a collection in MongoDB, we can use the find() method.

This method accepts a query object. If left empty, all documents will be returned.

```sh
db.posts.find()
```
findOne()
To select only one document, we can use the findOne() method.

This method accepts a query object. If left empty, it will return the first document it finds.

Note: This method only returns the first match it finds.

```sh
db.posts.findOne()
```
Querying Data
To query, or filter, data we can include a query in our find() or findOne() methods.

```sh
db.posts.find( {category: "News"} )
```
Projection
Both find methods accept a second parameter called projection.

This parameter is an object that describes which fields to include in the results.

Note: This parameter is optional. If omitted, all fields will be included in the results.

```sh
db.posts.find({}, {title: 1, date: 1})
```
We will get an error if we try to specify both 0 and 1 in the same object.
```sh
db.posts.find({}, {title: 1, date: 0})
```

## Update Document
To update an existing document we can use the updateOne() or updateMany() methods.

The first parameter is a query object to define which document or documents should be updated.

The second parameter is an object defining the updated data.

updateOne()
The updateOne() method will update the first document that is found matching the provided query.

Let's see what the "like" count for the post with the title of "Post Title 1":

```sh
db.posts.find( { title: "Post Title 1" } ) 
```
Now let's update the "likes" on this post to 2. To do this, we need to use the $set operator.

```sh
db.posts.updateOne( { title: "Post Title 1" }, { $set: { likes: 2 } } ) 
```
Check the document again and you'll see that the "like" have been updated.

```sh
db.posts.find( { title: "Post Title 1" } ) 
```
Insert if not found
If you would like to insert the document if it is not found, you can use the upsert option.

Update the document, but if not found insert it:

```sh
db.posts.updateOne( 
  { title: "Post Title 5" }, 
  {
    $set: 
      {
        title: "Post Title 5",
        body: "Body of post.",
        category: "Event",
        likes: 5,
        tags: ["news", "events"],
        date: Date()
      }
  }, 
  { upsert: true }
)
```
updateMany()
The updateMany() method will update all documents that match the provided query.

Update likes on all documents by 1. For this we will use the $inc (increment) operator:

```sh
db.posts.updateMany({}, { $inc: { likes: 1 } })
```
Now check the likes in all of the documents and you will see that they have all been incremented by 1.

## Delete Documents
We can delete documents by using the methods deleteOne() or deleteMany().

These methods accept a query object. The matching documents will be deleted.

deleteOne()
The deleteOne() method will delete the first document that matches the query provided.

```sh
db.posts.deleteOne({ title: "Post Title 5" })
```
deleteMany()
The deleteMany() method will delete all documents that match the query provided.

```sh
db.posts.deleteMany({ category: "Technology" })
```

## Operators
There are many query operators that can be used to compare and reference document fields.

Comparison
The following operators can be used in queries to compare values:

* $eq: Values are equal
* $ne: Values are not equal
* $gt: Value is greater than another value
* $gte: Value is greater than or equal to another value
* $lt: Value is less than another value
* $lte: Value is less than or equal to another value
* $in: Value is matched within an array

Logical
The following operators can logically compare multiple queries.

* $and: Returns documents where both queries match
* $or: Returns documents where either query matches
* $nor: Returns documents where both queries fail to match
* $not: Returns documents where the query does not match

Evaluation
The following operators assist in evaluating documents.

* $regex: Allows the use of regular expressions when evaluating field values
* $text: Performs a text search
* $where: Uses a JavaScript expression to match documents

## Update Operators
There are many update operators that can be used during document updates.

Fields
The following operators can be used to update fields:

* $currentDate: Sets the field value to the current date
* $inc: Increments the field value
* $rename: Renames the field
* $set: Sets the value of a field
* $unset: Removes the field from the document

Array
The following operators assist with updating arrays.

* $addToSet: Adds distinct elements to an array
* $pop: Removes the first or last element of an array
* $pull: Removes all elements from an array that match the query
* $push: Adds an element to an array

## Aggregation Pipelines
Aggregation operations allow you to group, sort, perform calculations, analyze data, and much more.

Aggregation pipelines can have one or more "stages". The order of these stages are important. Each stage acts upon the results of the previous stage.

```sh
db.posts.aggregate([
  // Stage 1: Only find documents that have more than 1 like
  {
    $match: { likes: { $gt: 1 } }
  },
  // Stage 2: Group documents by category and sum each categories likes
  {
    $group: { _id: "$category", totalLikes: { $sum: "$likes" } }
  }
])
```
Sample Data
To demonstrate the use of stages in a aggregation pipeline, we will load sample data into our database.

From the MongoDB Atlas dashboard, go to Databases. Click the ellipsis and select "Load Sample Dataset". This will load several sample datasets into your database.

## Aggregation $group
This aggregation stage groups documents by the unique _id expression provided.

Don't confuse this _id expression with the _id ObjectId provided to each document.

In this example, we are using the "sample_airbnb" database loaded from our sample data in the Intro to Aggregations section.

```sh
db.listingsAndReviews.aggregate(
    [ { $group : { _id : "$property_type" } } ]
)
```
This will return the distinct values from the property_type field.

## Aggregation $limit
This aggregation stage limits the number of documents passed to the next stage.

In this example, we are using the "sample_mflix" database loaded from our sample data in the Intro to Aggregations section.

```sh
db.movies.aggregate([ { $limit: 1 } ])
```
This will return the 1 movie from the collection.

## Aggregation $project
This aggregation stage passes only the specified fields along to the next aggregation stage.

This is the same projection that is used with the find() method.

In this example, we are using the "sample_restaurants" database loaded from our sample data in the Intro to Aggregations section.

```sh
db.restaurants.aggregate([
  {
    $project: {
      "name": 1,
      "cuisine": 1,
      "address": 1
    }
  },
  {
    $limit: 5
  }
])
```
This will return the documents but only include the specified fields.

Notice that the _id field is also included. This field is always included unless specifically excluded.

We use a 1 to include a field and 0 to exclude a field.

Note: You cannot use both 0 and 1 in the same object. The only exception is the _id field. You should either specify the fields you would like to include or the fields you would like to exclude.

## Aggregation $sort
This aggregation stage groups sorts all documents in the specified sort order.

Remember that the order of your stages matters. Each stage only acts upon the documents that previous stages provide.

In this example, we are using the "sample_airbnb" database loaded from our sample data in the Intro to Aggregations section.

```sh
db.listingsAndReviews.aggregate([ 
  { 
    $sort: { "accommodates": -1 } 
  },
  {
    $project: {
      "name": 1,
      "accommodates": 1
    }
  },
  {
    $limit: 5
  }
])
```
This will return the documents sorted in descending order by the accommodates field.

The sort order can be chosen by using 1 or -1. 1 is ascending and -1 is descending.

## Aggregation $match
This aggregation stage behaves like a find. It will filter documents that match the query provided.

Using $match early in the pipeline can improve performance since it limits the number of documents the next stages must process.

In this example, we are using the "sample_airbnb" database loaded from our sample data in the Intro to Aggregations section.

```sh
db.listingsAndReviews.aggregate([ 
  { $match : { property_type : "House" } },
  { $limit: 2 },
  { $project: {
    "name": 1,
    "bedrooms": 1,
    "price": 1
  }}
])
```
This will only return documents that have the property_type of "House".

## Aggregation $addFields
This aggregation stage adds new fields to documents.

In this example, we are using the "sample_restaurants" database loaded from our sample data in the Intro to Aggregations section.

```sh
db.restaurants.aggregate([
  {
    $addFields: {
      avgGrade: { $avg: "$grades.score" }
    }
  },
  {
    $project: {
      "name": 1,
      "avgGrade": 1
    }
  },
  {
    $limit: 5
  }
])
```
This will return the documents along with a new field, avgGrade, which will contain the average of each restaurants grades.score.

## Aggregation $count
This aggregation stage counts the total amount of documents passed from the previous stage.

In this example, we are using the "sample_restaurants" database loaded from our sample data in the Intro to Aggregations section.

```sh
db.restaurants.aggregate([
  {
    $match: { "cuisine": "Chinese" }
  },
  {
    $count: "totalChinese"
  }
])
```
This will return the number of documents at the $count stage as a field called "totalChinese".

## Aggregation $lookup
This aggregation stage performs a left outer join to a collection in the same database.

There are four required fields:

* from: The collection to use for lookup in the same database
* localField: The field in the primary collection that can be used as a unique identifier in the from collection.
* foreignField: The field in the from collection that can be used as a unique identifier in the primary collection.
* as: The name of the new field that will contain the matching documents from the from collection.

In this example, we are using the "sample_mflix" database loaded from our sample data in the Intro to Aggregations section.

```sh
db.comments.aggregate([
  {
    $lookup: {
      from: "movies",
      localField: "movie_id",
      foreignField: "_id",
      as: "movie_details",
    },
  },
  {
    $limit: 1
  }
])
```
This will return the movie data along with each comment.

## Aggregation $out
This aggregation stage writes the returned documents from the aggregation pipeline to a collection.

The $out stage must be the last stage of the aggregation pipeline.

In this example, we are using the "sample_airbnb" database loaded from our sample data in the Intro to Aggregations section.

```sh
db.listingsAndReviews.aggregate([
  {
    $group: {
      _id: "$property_type",
      properties: {
        $push: {
          name: "$name",
          accommodates: "$accommodates",
          price: "$price",
        },
      },
    },
  },
  { $out: "properties_by_type" },
])
```
The first stage will group properties by the property_type and include the name, accommodates, and price fields for each. The $out stage will create a new collection called properties_by_type in the current database and write the resulting documents into that collection.