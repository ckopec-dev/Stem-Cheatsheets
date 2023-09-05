# NoSQL Cheatsheet

## Key-value databases

- Simplest
- Get/set values by key
- Keys should be unique and fairly short
- Common key naming convention is to separate specicifity by colons, e.g. user:id:preferences

### Popular examples

- Redis
- DynamoDB
- Riak
- Project Voldemort

### Advantages

- Very simple + fast (no need for schema, types, etc)
- Only 3 operations:
  - Put: inserts new or updates existing key-value tuple
  - Get: returns a key's value
  - Delete: deletes the tuple
- Flexible (types are easily changed and attributes easily added)
- Scalability (horizontal scaling with sharding)

### Disadvantages

- Searching/querying is limited (need to know the key you're looking for)

### Some use cases

- User sessions
- User profiles and preferences
- Shopping carts
- Real-time recommendations
- Advertising

## Document databases

- Data is stored as documents (key-value pairs)
- Documents are grouped in collections
- Schemaless
- Formats: JSON, BSON, YAML, XML

### Popular examples

- mongoDB
- DyanamoDB
- Couchbase
- Firebase

### Advantages

- Schemaless
- No need to use joins
- Intuitive
- Horizontal scalability with sharding

### Disadvantages

- Code has to handle data validation
- Doesn't prevent redundant data

### Some use cases

- Catalogs
- Event logging
- Content management systems
- Real-time analytics

## Column family databases

- Derived from Google BigTable
- Data is stored in column families
- Also called "wide column databases"
- Consist of rows and columns
  - Each row can have different numbers of columns
  - Columns can be added when needed
  - Columns consist of: name, value, and timestamp (when data was inserted)

### Popular examples

- Cassandra
- Apache Hbase
- Accumulo

### Advantages

- Avoids default values
- Flexibility (add new columns as needed)
- Speed (related columns are stored together on disk)
- Scales horizontally with sharding
- Handles large quantity of data

### Disadvantages

- No multirow transactions
- No joins
- No subqueries

### Some use cases

- Large volumes of data
- Extreme write speeds
- Event logging
- Content management systems
- Time-series data

## Graph databases

- Treat data and relationships with same importance
- Based on mathematical graph theory
- Consist of vertices (nodes) and edges (links)
- Vertices represent entities (e.g. users, accounts, contacts)
- Edges represent relationships, and can have properties

### Popular examples

### Advantages

### Disadvantages

### Some use cases
