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

## Column family databases

## Graph databases

