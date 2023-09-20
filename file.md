#SQL Concepts - Technical Paper

## Table of Contents

1. [ACID](#acid)
2. [CAP THEOREM](#cap)
3. [Joins](#joins)
4. [Aggregations, Filters in queries](#ANF)
5. [Normalization](#normalisation)
6. [Indexes](#index)
7. [Transactions](#transaction)
8. [Locking mechanism](#locking)
9. [Database Isolation Levels](#isolation)
10. [Triggers](#trigger)

<a id="acid"></a># ACID

> ---
>
> "ACID" stands for Atomicity, Consistency, Isolation, and Durability
> ACID properties keep check for characteristics that ensure the reliability and consistency of database transactions.
>
> **1. Atomicity** :- Atomicity ensures that a database transaction is treated as a single, indivisible unit of work. It means that either all the operations within a transaction are completed successfully, or none of them are. There is no in-between state.
> **2. Consistency** :- Consistency ensures that a database remains in a valid state before and after a transaction. In other words, a transaction should bring the database from one consistent state to another.
> **3. Isolation** :- Isolation ensures that concurrent transactions do not interfere with each other. Transactions should be isolated from each other to prevent conflicts and maintain data integrity.
> **4. Durability** :- Durability ensures that once a transaction is committed, its changes are permanent and will survie any system failures, such as power outages or crashes.
>
> ---

<a id="cap"></a># CAP THEOREM

> ---
>
> The CAP theorem, originally introduced as the CAP principle, can be used to explain some of the competing requirements in a distributed system with replication. It is a tool used to make system designers aware of the trade-offs while designing networked shared-data systems.
>
> The three letters in CAP refer to three desirable properties of distributed systems with replicated data i.e. _consistency_ , _availability_ , _partition tolerance_.
>
> **Cap Theroem** states that it is not possible to aplly all three of these desirable properties at the same time in a distributed system with data replication.
>
> **1. Consistency**:-
>
> > Consistency means that the nodes will have the same copies of a replicated data item visible for various transactions. A guarantee that every node in a distributed cluster returns the same, most recent and a successful write. Consistency refers to every client having the same view of the data.
>
> **2. Availability**:-
>
> > Availability means that each read or write request for a data item will either be processed successfully or will receive a message that the operation cannot be completed. Every non-failing node returns a response for all the read and write requests in a reasonable amount of time.
>
> **3. Partition Tolerance**:-
>
> > Partition tolerance means that the system can continue operating even if the network connecting the nodes has a fault that results in two or more partitions, where the nodes in each partition can only communicate among each other. That means, the system continues to function and upholds its consistency guarantees in spite of network partitions. Network partitions are a fact of life. Distributed systems guaranteeing partition tolerance can gracefully recover from partitions once the partition heals.
>
> The following figure represents which database systems prioritize specific properties at a given time:
>
> **1. CA(Consistency and Availability)**:-
>
> > - The system prioritizes availability over consistency and can respond with possibly stale data.
> > - Example databases: Cassandra, CouchDB, Riak, Voldemort.
>
> **2. AP(Availability and Partition Tolerance)**:-
>
> > - The system prioritizes availability over consistency and can respond with possibly stale data.
> > - The system can be distributed across multiple nodes and is designed to operate reliably even in the face of network partitions.
> > - Example databases: Amazon DynamoDB, Google Cloud Spanner.
>
> **2. CP(Consistency and Partition Tolerance)**:-
>
> > - The system prioritizes consistency over availability and responds with the latest updated data.
> > - The system can be distributed across multiple nodes and is designed to operate reliably even in the face of network partitions.
> > - Example databases: Apache HBase, MongoDB, Redis.
>
> ---

<a id="joins"></a># Joins

> ---
>
> Joins are operations used to combine rows from two or more tables based on a related column between them. SQL joins are essential for retrieving data from multiple tables in a relational database.
> Types of Joins :-
>
> **1. INNER JOIN**: Returns only the rows that have matching values in both tables.
>
> ```js
> SELECT columns
> FROM table1
> INNER JOIN table2 ON table1.column = table2.column;
> ```
>
> **2. LEFT OUTER JOIN**: Returns all rows from the left table (table1), and the matched rows from the right table (table2). If there are no matches, it returns NULL for right table columns.
>
> ```js
> SELECT columns
> FROM table1
> LEFT JOIN table2 ON table1.column = table2.column;
> ```
>
> **3. RIGHT OUTER JOIN**: Opposite of LEFT JOIN. Returns all rows from the right table (table2), and the matched rows from the left table (table1). If there are no matches, it returns NULL for left table columns.
>
> ```js
> SELECT columns
> FROM table1
> RIGHT JOIN table2 ON table1.column = table2.column;
> ```
>
> **4. FULL OUTER JOIN**: Returns all rows when there is a match in either the left or right table. If there's no match, it returns NULL for columns from the table with no match.
>
> ```js
> SELECT columns
> FROM table1
> FULL JOIN table2 ON table1.column = table2.column;
> ```
>
> **5. SELF JOIN**: Returns the Cartesian product of rows from both tables, meaning it combines each row from the first table with every row from the second table.
>
> ```js
> SELECT columns
> FROM table1 t1
> INNER JOIN table1 t2 ON t1.column = t2.column;
> ```
>
> **6. CROSS JOIN**: Returns the Cartesian product of rows from both tables, meaning it combines each row from the first table with every row from the second table.
>
> ```js
> SELECT columns
> FROM table1
> CROSS JOIN table2;
> ```
>
> > ---

<a id="ANF"></a>#Aggregations and Filters

> ---
>
> 1. **Aggregations**: Aggregations are used to perform calculations on sets of rows in a database table, often to summarize or compute statistical information. Common aggregation functions include:
>    ####
>    1.**Count**: Counts the number of rows that match a specified condition.
>    ```js
>    SELECT COUNT(*) FROM employees WHERE department = 'Sales';
>    ```
>    2.**SUM**: Calculates the sum of numeric values in a column.
>    ```js
>    SELECT SUM(salary) FROM employees;
>    ```
>    3.**AVG**:Computes the average of numeric values in a column.
>    ```js
>    SELECT AVG(age) FROM students;
>    ```
>    4.**MIN**: Finds the minimum value in a column.
>    ```js
>    SELECT MIN(price) FROM products;
>    ```
>    5.**MAX**: Finds the maximum value in a column.
>    ```js
>    SELECT MAX(score) FROM exam_results;
>    ```
>    6.**GROUP BY**:Groups rows based on one or more columns and applies aggregation functions to each group.
>    ```js
>    SELECT department, AVG(salary) as avg_salary FROM employees GROUP BY department;
>    ```
>
> ##
>
> 2. **Filters**: Filters in SQL queries allow you to specify conditions to retrieve a subset of rows that meet certain criteria. Common ways to filter data include:
>    ####
>    1.**WHERE Clause:**: The WHERE clause is used to filter rows based on specific conditions.
>    ```js
>    SELECT * FROM products WHERE price < 50;
>    ```
>    2.**AND & OR Operators**: You can use AND and OR operators to combine multiple conditions in a WHERE clause.
>    ```js
>    SELECT * FROM customers WHERE age > 25 AND city = 'New York';
>    ```
>    3.**IN Operator**: Checks if a value matches any value in a list.
>    ```js
>    SELECT * FROM orders WHERE status IN ('Shipped', 'Delivered');
>    ```
>    4.**BETWEEN Operator**: Filters rows with values within a specified range.
>    ```js
>    SELECT * FROM sales WHERE sale_date BETWEEN '2023-01-01' AND '2023-12-31';
>    ```
>    5.**LIKE Operator**:Searches for patterns in text data using wildcard characters.
>    ```js
>    SELECT * FROM products WHERE product_name LIKE 'App%';
>    ```
>    6.**NULL Values**: You can filter rows with NULL or non-NULL values.
>    ```js
>    SELECT * FROM employees WHERE manager_id IS NULL;
>    ```
>    7.**ORDER BY Clause**: While not a filter per se, you can use ORDER BY to sort the results of your query in ascending or descending order based on one or more columns.
>    ```js
>    SELECT * FROM students ORDER BY last_name ASC;
>    ```
>    Combining aggregations and filters in SQL queries allows you to retrieve specific subsets of data and perform calculations on that data. This capability is crucial for data analysis, reporting, and extracting meaningful insights from your database.
>
> ---

<a id="normalisation"></a># Normalization

> ---
>
> Normalization is a process in database design that aims to eliminate data redundancy and improve data integrity by organizing data into separate related tables.
>
> 1.  **First Normal Form (1NF)** :-
>     - Ensures that each table has a primary key and that each column contains only atomic (indivisible) values.
>     - Eliminates repeating groups by breaking them into separate rows.
> 2.  **Second Normal Form (2NF)** : -
>     - Requires that the table be in 1NF and that no non-key attribute (column) be partially dependent on the primary key.
>     - Involves splitting tables to separate related data and creating new tables with foreign keys.
> 3.  **Third Normal Form (3NF)** :-
>     - Requires that the table be in 2NF and that there are no transitive dependencies between non-key attributes.
>     - Further eliminates data redundancy by ensuring that non-key attributes depend only on the primary key.
> 4.  **Boyce-Codd Normal Form (BCNF)** : -
>     - A stricter version of 3NF that requires that every non-trivial functional dependency in the table is a superkey.
>     - Helps eliminate anomalies more effectively but may require additional table restructuring.
> 5.  **Fourth Normal Form (4NF)** :-
>     - Addresses multi-valued dependencies by ensuring that there are no non-trivial multi-valued dependencies.
>     - Typically used in more complex database schemas with many-to-many relationships.
> 6.  **Fifth Normal Form (5NF):** : -
>     - Deals with join dependencies, which occur when a table can be reconstructed by joining multiple smaller tables.
>     - Often used in advanced database design scenarios.
>
> Over-normalization can lead to performance issues, as it may require complex joins to retrieve data, so it should be balanced with the specific needs of the application and the query patterns. Denormalization, or selectively relaxing normalization rules, can sometimes be necessary for performance optimization in certain situations.
>
> ---

<a id="index"></a># Indexes

> ---
>
> Indexes in databases are structures that speed up data retrieval. They're created on specific columns, allowing the system to quickly locate rows based on criteria. Primary keys and unique indexes enforce data uniqueness. Indexes come in clustered (determines data order) and non-clustered (pointers to data) types. While they enhance read performance, they increase storage and maintenance costs for write operations. Proper index selection and maintenance are crucial for query optimization.
>
> ---

<a id="transaction"></a># Transactions

> ---
>
> Transactions in a database are sequences of one or more SQL operations that are treated as a single, indivisible unit of work. They ensure data consistency, integrity, and reliability in a database system. Here are key points about transactions:
>
> 1.  **Atomicity** :-
>     - Transactions follow the "ACID" properties, with "A" standing for Atomicity.
>     - Atomicity ensures that all the operations within a transaction are completed successfully or none are. There's no in-between state.
> 2.  **Consistency** :-
>     - Transactions bring the database from one consistent state to another.
>     - Data integrity constraints are enforced, preventing violations of business rules.
> 3.  **Isolation** :-
>     - Isolation ensures that concurrent transactions do not interfere with each other.
>     - Different isolation levels (e.g., Read Uncommitted, Serializable) provide varying levels of isolation.
> 4.  **Durability** :-
>     - Durability guarantees that once a transaction is committed, its changes are permanent and survive system failures.
>     - Changes are persistently stored in non-volatile storage.
> 5.  **Begin, Commit, and Rollback** :-
>     - Transactions are typically initiated with a "BEGIN" statement, followed by a series of SQL operations.
>     - They can be committed with a "COMMIT" statement, making changes permanent, or rolled back with a "ROLLBACK" statement, undoing changes.
> 6.  **Concurrency Control** :-
>     - Transaction managers use concurrency control mechanisms to handle simultaneous transactions.
>     - Locking, timestamps, and other techniques prevent conflicts and maintain data integrity.
> 7.  **Transaction States:** :-
>     - Transactions go through various states, including "Active," "Partially Committed," and "Committed."
>     - If an error occurs, they may enter the "Rollback" state to undo changes.
> 8.  **Nested Transactions** :-
>     - Some database systems support nested transactions, allowing transactions to be divided into subtransactions.
>     - Outer transactions can be committed or rolled back independently of inner transactions.
> 9.  **Savepoints** :-
>     - Savepoints within a transaction enable rolling back to a specific point in the transaction without undoing all changes.
>
> Transactions are fundamental for ensuring data integrity and consistency in databases, especially in multi-user or concurrent access environments. They guarantee that the database remains in a reliable state, even in the presence of errors or system failures.
>
> ---

<a id="locking"></a># Locking mechanism

> ---
>
> A locking mechanism in a database management system (DBMS) is a technique used to control access to data and ensure data consistency in a multi-user or concurrent environment. Locks are used to prevent conflicts when multiple transactions attempt to access or modify the same data simultaneously. Here are few points about locking mechanisms:
>
> 1.  **Purpose of Locking** :-
>     - Locking ensures that only one transaction can access or modify a piece of data at a time, preventing concurrent transactions from interfering with each other.
>     - It helps maintain data integrity and consistency in a multi-user database system.
> 2.  **Lock Duration** :-
>     - Locks can be categorized as short-term (held for the duration of a single SQL statement) or long-term (held for the duration of a transaction).
>     - Long-term locks can lead to blocking and should be released as soon as possible to minimize contention.
> 3.  **Deadlocks** :-
>     - A deadlock occurs when two or more transactions are waiting for each other to release locks, resulting in a standstill.
>     - DBMSs implement deadlock detection and resolution mechanisms to break deadlocks.
> 4.  **Optimistic Concurrency Control:** :-
>     - DBMSs use optimistic concurrency control, where locks are acquired only when a transaction is ready to commit. Conflicts are detected at that point, and the transaction may need to be rolled back and retried.
>
> Locking mechanisms are essential for ensuring data consistency and preventing data anomalies in a multi-user database environment. The choice of locking strategy and isolation level depends on the specific requirements and performance considerations of the application.
>
> ---

<a id="isolation"></a># Isolation Levels

> ---
>
> Isolation levels in a database management system (DBMS) determine the level of visibility and interaction between concurrent transactions. Different isolation levels provide varying degrees of data consistency and isolation to meet the requirements of different applications. There are four standard isolation levels defined by the SQL standard:
>
> 1.  **Read Uncommitted** :-
>     - The lowest isolation level.
>     - Allows a transaction to read uncommitted changes made by other transactions.
>     - No locks are acquired on read operations, leading to minimal contention.
>     - Prone to dirty reads, non-repeatable reads, and phantom reads.
> 2.  **Read Committed** :-
>     - Default isolation level in many DBMSs.
>     - A transaction can only read committed changes made by other transactions.
>     - Prevents dirty reads but allows non-repeatable reads and phantom reads.
>     - Acquires shared locks on read operations, potentially causing some blocking.
> 3.  **Repeatable Read** :-
>     - Ensures that a transaction can read the same data consistently throughout the transaction.
>     - Prevents dirty reads and non-repeatable reads but allows phantom reads.
>     - Acquires shared locks on read operations, leading to more potential blocking.
> 4.  **Serializable** :-
>     - The highest isolation level.
>     - Ensures complete data consistency by preventing all anomalies (dirty reads, non-repeatable reads, phantom reads).
>     - Acquires the most locks, leading to significant potential for blocking and reduced concurrency.
>     - Often requires explicit transaction control commands, such as locking specific rows or using serializable transactions.
>
> In addition to these standard isolation levels, some database systems offer vendor-specific isolation levels or customizable levels. These allow developers to fine-tune isolation characteristics to meet application-specific requirements.
> When choosing an isolation level, consider factors such as the requirements of your application, the level of concurrency needed, and the potential impact on performance. It's essential to strike a balance between data consistency and system performance, as higher isolation levels often lead to increased contention and locking, which can affect throughput.
>
> ---

<a id="trigger"></a># Triggers

> ---
>
> Triggers in a database are special types of stored procedures or functions that are automatically invoked in response to specific events or actions that occur within the database. They are used to enforce data integrity, automate tasks, and maintain data consistency.
>
> #### syntax:
>
> ```sql
>    create trigger [trigger_name]
>
>    [before | after]
>
>    {insert | update | delete}
>
>    on [table_name]
>
>    [for each row]
>
>    [trigger_body]
> ```
>
> ---
