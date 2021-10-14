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
