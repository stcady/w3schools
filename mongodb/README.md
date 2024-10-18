# MongoDB

## Create Collection using mongosh
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


