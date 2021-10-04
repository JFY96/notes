# CRUD

`CRUD` is a concept that comes up a lot in web development. It stands for *Create*, *Read*, *Update* and *Delete*. These are the 4 basic functions that are built into database driven apps. 

The CRUD Operations roughly correlate to HTTP methods:

- `create` - `POST`
- `read` - `GET`
- `update` - `PUT`
- `delete` - `DELETE`

# MySQL with NodeJS

Can connect Node.js app via a package like mysql2:
```js
const mysql = require("mysql2");

// Connection Pool
const pool = mysql.createPool({
    host: "localhost",
    user: "root",
    database: "db",
    password: "password"
});

modules.exports = pool.promise();
```

```js
const db = require("./database.js");

// Simple query
db.execute("SELECT * FROM table")
    .then([rows, fieldData] => {
        // data/results is in rows
        // fieldData contains extra meta data about the results if available
    })
    .catch(err => {
        
    });

// Select with simple WHERE
db.execute("SELECT * FROM items WHERE items.id = ?", [id]);

// Simple insert
// Note "desc" is a reserved command in mysql so need to wrap it in back ticks (`)
db.execute(
    "INSERT INTO items(title, `desc`, imageUrl) VALUES(?,?,?)",
    [title, description, imageUrl]
);
```

# Sequelize

- A library in JavaScript that makes it easy to manage a SQL database.
- It is an **ORM** / Object-Relational Mapper (it maps an object syntax onto database schemas).
- Sequelize allows you to define models and interact with the database through them.

## Core Concepts

- Models (e.g. `User, Item`)
- Instances (`const user = User.build()`)
- Queries (`User.findAll()`)
- Associations (`User.hasMany(Item)`)

## Model

A model is an abstraction that represents a table in the database.

```js
// defining a model
const Item = sequelize.define("item", {
    id: {
        type: Sequelize.INTEGER,
        autoIncrement: true,
        allowNull: false,
        primaryKey: true
    },
    title: Sequelize.STRING,
    desc: {
        type: Sequelize.STRING,
        allowNull: false,
    }
});
```

If a table name isn't given, it will automatically use the pluralized version of the model name (so `items` for the above, `people` for `person`). There are options to enforce the table name to be the model name (`freezeTableName`), and option to provide the table name directly (`tableName`)

```js
sequelize.define('User', {
  // ... (attributes)
}, {
  tableName: 'Employees'
});
```

When defining a model, the table may not exist in the database yet. A model can be synchronized with the database by calling `model.sync(options)`. This will perform an SQL query and create the table if it doesn't already exist (can be forced depending on options passed - i.e for development).

To sync all models at once, use `sequelize.sync()`.

## Insert Queries

`Item.create({id: id, title: title, desc: desc});`

## Select Queries

`Item.findAll()`

## Update Query

`item.save()`

## Delete Query

`item.destroy()`

## Associations

Associations between models are like relations between tables.

### One-to-One

```js
// One-To-One relationship between A and B, with the foreign key defined in the model B
A.hasOne(B, {options}); // Source Model A, Target Model B
B.belongsTo(A, {options});
```

### One-to-Many

```js
// One-To-Many relationship between A and B, with the foreign key defined in the model B
A.hasMany(B, {options});
B.belongsTo(A, {options});
```

### Many-to-Many

```js
// Many-To-Many relationship between A and B, using C as junction table, which will have the foreign keys (e.g. aId, bId)
A.belongsToMany(B, { through: 'C' });
// Note: Sequelize will automatically generate the model C, unless the passed model C is already defined
```

### Eager Loading

For querying data of several models at once - one main model and one or more associated models, like joins in traditional SQL.
Performed by using the `include` option.

```js
// getting a users orders with the products data
User.getOrders({include: ["products"]})
```

# MongoDB

Alternative to SQL databases. No strict schemes, fewer relations.

NoSQL database have collections (equivalent to tables), and documents inside collections (instead of records).

MongoDB uses JSON data format to store documents in collections (actually BSON - binary encoding of JSON-like documenta for MangoDB).

For relations between documents, we can embed documents inside documents (duplicate data) or use references (i.e just store the ids). Documents in documents are called embedded (or nested) documents. 


# Working with MongoDB (nodeJS)

- Commands like `insertOne()`, `find()`, `updateOne()`, `deleteOne()` make CRUD operations very simple

- All operations are promise-based, so they can be easily chained for more complex flows.

- MongoDB stores ids in documents as `_id` and it is not a regular string. 
To search using the id: 
```js
return db.collection("products")
    .find({_id: mongodb.ObjectId(productId)})
    // note this returns a cursor object (not the actual documents), which allows you to iterate through the documents
    .toArray();
    // .toArray() can be used to iterate over the cursor, load documents into RAM, and exhaust the cursor object - not the best practice for large data sets.
```

```js
.next() // moves the cursor (returned by find()) to the next document
```

# Mongoose

- A ODM (Object-Document-Mapping) Library for MongoDB and Node.js
- It manages relationships between data, provides schema validation, and is used to translate between objects in code and the representation of those objects in MongoDB

## Connecting to MongoDB database

```js
// Import the mongoose module
const mongoose = require('mongoose');

// Set up default mongoose connection
const mongoDB = 'mongodb://127.0.0.1/my_database'; // uri for your MongoDB cluster 
mongoose.connect(mongoDB, { useNewUrlParser: true, useUnifiedTopology: true });

// Get the default connection
const db = mongoose.connection;

// Bind connection to error event (to get notification of connection errors)
db.on('error', console.error.bind(console, 'MongoDB connection error:'));
```

## Core Concepts

- Schemas & Models
- Instances
- Queries

## Defining a Schema

```js
const mongoose = require('mongoose');

const Schema = mongoose.Schema;

const userSchema = new Schema({
    name: String,
    email: String,
	 dateRegistered: Date,
    items: [{
        itemId: { type: Schema.Types.ObjectId, ref: 'Item' } // Join/Link to another model
    }],
});
```

## Creating a model

Models are created from schemas using `mongoose.model()` method

First argument is the singular name of the collection that will be created for your model 

Second argument is the schema to be used in created the model

```js
// (continuation from above)
mongoose.model('User', userSchema); // will automatically create a mongoDB collection with lowercase name defined
```

## Schema types (fields)

A schema can have an arbitrary number of fields and each one represents a field in the documents stored in MongoDB. 

Here is an example with some common field types:
```js
const schema = new Schema({
	name: String,
	binary: Buffer,
	living: Boolean,
	updated: {
		type: Date,
		default: Date.now(),
	},
	age: {
		type: Number,
		min: 18,
		max: 65,
		required: true
	},
	mixed: Schema.Types.Mixed, // Arbitrary schema type
	_someId: Schema.Types.ObjectId,
	array: [], // 
	ofString: [String],
	nested: {
		stuff: {
			type: String,
			lowercase: true,
			trim: true
		}
	}
});
```

`ObjectId` represents specific instances of a model in the database. E.g. a book might use this to represent its author object (it will contain the unique id `_id` for the specified object)

Arrays - you can have an array of objects without a specified type (`[]`) and an array of a specific type of object (`String` in the example above).

As shown in the example you can either declare a field with name/type as a key-value pair, or have an object following the name with additional options such as:
- default values
- built-in validators (e.g. min/max) and custom validation functions
- if field is required
- For `String` fields, if they should automatically be set to lowercase/uppercase/trimmed (e.g. `{ type: String, lowercase: true, trim: true }`)

## Validation

- Mongoose provides built-in and custom validators, and sync/async validators
- Some built-in:
	- `required`
	- `Numbers` have `min` and `max`
	- `Strings` have
		- `enum` - specifies a set of allowed values for the field
		- `match` - specifies RegEx that the string must match
		- `maxLength` and `minLength`

You can also define a error message for validation failures:

```js
someNumber: {
	min: [2, 'Too small'],
	max: 10,
}
```

## Virtual properties

These are document properties that you can get/set but do not get persisted to MongoDB. For example:

```js
const personSchema = new Schema({
	name: {
		first: String,
		last: String
	}
});

personSchema.virtual('fullName').
	get(function() {
		return this.name.first + ' ' + this.name.last;
	}).
	set(function(v) {
		this.name.first = v.substr(0, v.indexOf(' '));
		this.name.last = v.substr(v.indexOf(' ') + 1);
	});
```

## Operations

- Operations (creation, updates, deletes, queries) are asynchronous operations
	- You supply a callback or chain on the promise returned
		- The API uses error-first argument convention, so the first argument for callback will always be an error value (or null). If the API returns a result, then this will be in the second argument

### Creating/Modifying documents

- To create a record you define an instance of the model then call `save()`
```js
const awesome_instance = new SomeModel({ name: 'awesome' });

awesome_instance.save(function (err) {
  if (err) return handleError(err);
  // saved!
});
```
- Can also use `create()` to define the model instance at the same time as you save it.
```js
SomeModel.create({ name: 'awesome' }, function (err, awesome_instance) {
  if (err) return handleError(err);
  // saved!
});
```
- After changing values in some fields, you can call `save()` or `update()` to store modified values back to db
```js
awesome_instance.name="New cool name";
awesome_instance.save(function (err) {
   if (err) return handleError(err); // saved!
});
```

### Searching for records

- Some common operations include `find()`, `findById()`, `findOne()`
- Convenience functions for updating and removing records: `findByIdAndRemove()`, `findByIdAndUpdate()`, `findOneAndRemove()`, `findOneAndUpdate()`

Example
```js
// find all athletes who play tennis, selecting the 'name' and 'age' fields
Athlete.find({ 'sport': 'Tennis' }, 'name age', function (err, athletes) {
  if (err) return handleError(err);
  // 'athletes' contains the list of athletes that match the criteria.
})
```

- if no callback is specified then the API will return a. object of type `Query`. You can then use this object to build up the query then execute it using `exec()`:
```js
const query = User.find({ 'country': 'UK' });

// select name, age, email, but exclude id
query.select("name age email -_id");

// limit results to 5 items
query.limit(5);

// sort by age
query.sort({ age: -1 });

// execute query
query.exec((err, athletes) => {
	if (err) return handleError(err);
});
```

- can also do something similar using `where()` and also can chain them using the `.` operator
```js
User.
	find().
	where('country').equals('UK').
	where('age').gt(18).lt(65).
	limit(5).
	sort({ age: -1 }).
	exec(callback);
```

###

- `_doc`

## Relations / Population

- You can create references from one document/model instance to another using `ObjectId` schema field, or one to many using an array of `ObjectId`s
```js
const mongoose = require('mongoose'), Schema = mongoose.Schema;

const authorSchema = Schema({
  name    : String,
  stories : [{ type: Schema.Types.ObjectId, ref: 'Story' }]
});

const storySchema = Schema({
  author : { type: Schema.Types.ObjectId, ref: 'Author' },
  title    : String
});

var Story  = mongoose.model('Story', storySchema);
var Author = mongoose.model('Author', authorSchema);
```
- Can save references to the related document by assigning the `_id` value

- If you need the actual content of the associated document, `populate()` allows you to populate the id for any linked models with the data of that model instead
    - for nested, chain with the dot operator
```js
User.find().populate("items.itemId").execPopulate().then(result => {});
```
- note `populate()` is not then-able, so will have to chain `execPopulate()` afterwards

## Methods

- Can add own methods to the schema
```js
userSchema.methods.methodName = function() {

}
```

### Instance methods

### Static methods

### Query helpers

## One schema/model per file

Recommended to define each model schema in its own module (file), then exporting the method to create the model
```js
const userModelSchema = new Schema({...});

module.exports = mongoose.model('UserModel', userSchema);
```

Then can require and use the model immediately in other files:

```js
// Create UserModel model just by requiring module
const UserModel = require('../models/UserModel');

// Use the model to find records
UserModel.find(callbackFunc);
```

## Form Design

If many models are related/dependent, there may be cases where a user wants to:
- Create an object when its related object do not exist yet
- Delete an object that is still being used by another object

A simple implementation may be stating that a form can only:
- Create an object using objects that already exist
- Delete an object if it is not referenced by other objects

## Pagination
