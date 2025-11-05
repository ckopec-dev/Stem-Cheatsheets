
# Normalization Cheatsheet

## First normal form

- A table must have a primary key.
- A column can only have a single data type.
- Information can not be contained in row order.
- Groups cannot be repeated. E.g. When storing contacting information for accounts, the contact info should be in a different table.

## Second normal form

- Each non-key column must be dependent on the entire primary key.

## Third normal form

- Each non-key column must depend on the entire primary key and nothing but the key.

## Fourth normal form

- The only multivalued dependencies allowed are multivalued dependencies on the key.

## Fifth normal form

- It must not be possible to describe the tables as being the logical result of joining other tables together.
