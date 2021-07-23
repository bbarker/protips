
# Cassandra
- a row only accumulates storage space for non-null values, so it is sparse, in a sense.
- partition key, then clustering column, divides up data.
- frequently, the primary key is composed of the partition key and contact time (I think); tables (tend to be?) denormalized
- foreign keys don't exist, as cassandra emphasizes query speed, and this would over complicate the data retrieval model
- static columns are always the same when the partitition key is the same; updating a static column updates the value for all rows of the same partition keys (it is actually shared)
- Types
  - Custom types composed of other types (like record types)
  - blob types for binary data
  - Collection types: Set, List, Map
  - UUID / Time UUID
- Batch commands let you group multiple writes together, but isn't transactional
  - Does support "lightweight" transactions: "IF NOT EXISTS" 
  - Data stored from batch can be read before the batch command is completely finished
- Can configure expiry options for data, so that it is GC'd at some point.

# cqlsh
- supports tab completion
- 

# Testing
- nosqlbench can be used for testing a db under load

![image](https://user-images.githubusercontent.com/916366/126727557-f1d0f07d-9ddc-49ce-b045-6b45b9b070c3.png)

![image](https://user-images.githubusercontent.com/916366/126727574-4aeb4a40-c815-4b25-95f3-394bf2023c24.png)
