
# Cassandra
- a row only accumulates storage space for non-null values, so it is sparse, in a sense.
- partition key, then clustering column, divides up data.
- frequently, the primary key is composed of the partition key and contact time (I think)
- static columns are always the same when the partitition key is the same; updating a static column updates the value for all rows of the same partition keys (it is actually shared)
- Types
  - Custom types composed of other types (like record types)
  - blob types for binary data
  - Collection types: Set, List, Map

# cqlsh
- supports tab completion
- 

# Testing
- nosqlbench can be used for testing a db under load
