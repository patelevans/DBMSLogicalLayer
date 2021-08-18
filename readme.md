# SQL Database Project

As part of a databases course, I implemented a logical to physical mapping for a SQL database built atop BerkeleyDB's key value store.
The core aim of the project was to create a SQL interface for BerkeleyDB instead of simply using the one that BerkeelyDB provides.
All of the code that I worked on is in:
- InsertRow.java
- TableIterator.java
- InsertStatement.java
- SelectStatement.java

All other files are part of the provided code.

I implemented marshalling for the data by making the primary key the key in the key-value pair, and then writing relevant metadata and data to the value. The value was structured as such:
- [Offset1, Offset2, Offset 3, ..., Value1, Value2, Value3, ...]
- Offsets represent the start points of each value in the byte array. Offsets for primary keys were the out of band value -1, and the offsets for null values were the out of band value -2.
- There number of offsets exceeds the number of values by 1, to store the offset of the end of the last value.
- This allows for ease of access to read any value, as for each value, we have a start and end offset in the byte array.

I also implemented the necessary logic to get the values stored in a specific column. This involved unmarshalling the previously marshalled data, and then returning the correct part of the bytes array from the function to then be used to read using the appropriate function for reading specific data types.

In addition, I implemented the core logic for inserting into and selecting from a table.