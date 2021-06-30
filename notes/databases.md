Databases - used to store data and make it easily accessible. Quicker access than with a file (e.g. JSON)

Types:
- SQL Database e.g. MySQL
- NoSQL Databases e.g. MongoDB

# SQL

- Has tables and rows in each table
- Strong Data Schema (clearly defined tables - all data has to fit & be of right type)
- Data Relations (Tables can be connected)
    - One-to-One
    - One-to-Many
    - Many-to-Many
- SQL Queries 

# NoSQL

- Has Collections (equivalent of SQL tables)
- Has Documents inside collections (equivalent of rows in SQL)
- Fields - similar to columns in SQL
- Schema-less (no strongly defined structure on what fields are needed)

- No/Few Data Relations
    - Can relate documents but don't have to (and shouldn't do it too much as it will make queries slow)
    - Duplicate data across collections instead of table relations
    - Much quicker for reading as no SQL table joins

# Horizontal vs Vertical Scaling

- Horizontal - Add More Servers (and merge Data into one Database)
- Vertical - Improve Server Capacity / Hardware

# Comparing SQL and NoSQL

| SQL | NoSQL |
| :--- | :--- |
| Data uses Schemas | Schema-less |
| Has Relations | Fewer Relations |
| Data is distributed across multiple tables | Data is typically merged / nested in a few collections |
| Horizontal scaling is difficult. Vertical scaling is possible | Both horizontal and vertical scaling is possible |
| Limitations for lots (thousands) read & write queries per second | Great performance for mass read & write requests |


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

## Core Concepts

- Schemas & Models
- Instances
- Queries

## Defining a Schema

```js
const userSchema = new mongoose.Schema({
    name: {
        type: String,
        required: true,
    },
    email: {
        type: String,
        required: true,
    },
    items: [{
        itemId: { type: Schema.Types.ObjectId, ref: "Item" } // Join/Link to another model
    }]
});

module.exports = mongoose.model("User", userSchema); // will automatically create a mongoDB collection with lowercase name defined
```

## Operations

- Some common operations include `find()`, `findById()`, `findByIdAndRemove()`, `save()`

- `select()` to get specific fields
```js
User.find().select("name email -_id") // get name, email, but exclude id
```

- `_doc`

## Relations / Populate

- `populate()` allows you to populate the id for any linked models with the data of that model instead
    - for nested e.g. the above, chain with the dot operator

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

## Pagination

