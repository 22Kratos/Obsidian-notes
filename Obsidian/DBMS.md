DBMS (Database Management System) is a software system that manages, stores, and retrieves data efficiently in a structured format.

- It allows users to create, update, and query databases efficiently.
- Ensures data integrity, consistency, and security across multiple users and applications.
- Reduces data redundancy and inconsistency through centralized control.
- Supports concurrent data access, transaction management, and automatic backups

![[Pasted image 20260123154525.png]]

# Problem with tradition file systems

- ****Data Redundancy:**** Duplicate entries across files
- ****Inconsistency:**** Conflicting or outdated information
- ****Difficult Access:**** Manual file search required
- ****Poor Security:**** No control over data access
- ****Single-User Access:**** No support for collaboration
- ****No Backup/Recovery:**** Data loss was often permanent

# Components

## Hardware

- Physical devices like servers, disks, input-output devices (keyboard, monitor, printer).
- Stores and processes data; interfaces between real-world inputs and digital systems.
- Examples: Personal computer hard disk, RAM, network devices used for DBMS operations.

## Software

- Actual DBMS software like MySQL, Oracle, PostgreSQL.
- Includes the database engine, OS, network software, and application tools.
- Translates database access languages into operations.

## Data

- Raw facts stored in structured or unstructured formats.
- ****Operational Data****: Actual user data (e.g., name, age).
- ****Metadata****: Data about data (e.g., storage time, size, data type).
- Core reason DBMS exists—to manage and store data efficiently.

## Procedures

- Instructions and rules for using DBMS effectively.
- Covers setup, login/logout, data validation, backup, access control, and report generation.
- Helps ensure consistent and secure use of the system.

## Database Access Language

- Used to interact with the database (create, read, update, delete data).
- Examples: SQL, MyAccess, Oracle PL/SQL.
- ****DDL (Data Definition Language)**** – `CREATE`, `ALTER`, `DROP`
- ****DML (Data Manipulation Language)**** – `INSERT`, `UPDATE`, `DELETE`

## People

- Users interacting with DBMS at different levels:
- ****Database Administrators (DBA)**** – Manage security, performance, user access.
- ****Developers**** – Build applications using the database.
- ****End Users**** – Use applications to access the database (e.g., students, employees).

# Types of DBMS

## Relational DBMS

- It organizes data into tables (relations) composed of rows and columns.
- Uses primary keys to uniquely identify rows and foreign keys to establish relationships between tables.
- Queries are written in ****SQL (Structured Query Language)****, which allows for efficient data manipulation and retrieval.

****Examples:**** MySQL oracle, Microsoft SQL Server and Postgre SQL.

## NoSQL DBMS

- They are designed to handle large-scale data and provide high performance for scenarios where relational models might be restrictive.
- They store data in various non-relational formats, such as key-value pairs, documents, graphs or columns.
- These flexible data models enable rapid scaling and are well-suited for unstructured or semi-structured data.

****Examples****: MongoDB, Cassandra, DynamoDB and Redis.

## Object-Orientated DBMS

- It integrates object-oriented programming concepts into the database environment, allowing data to be stored as objects.
- Supports complex data types and relationships, making it ideal for applications requiring advanced data modeling and real-world simulations.

****Examples****: ObjectDB, db4o.

## Hierarchical Database

- Organizes data in a tree-like structure, where each record (node) has a single parent and have multiple children.
- This model is similar to a file system with folders and subfolders.
- It is efficient for storing data with a clear hierarchy, such as organizational charts or file directories.
- Navigation is fast and predictable due to the fixed structure.
- It lacks flexibility and difficult to restructure or handle complex many-to-many relationships.

****Example:**** IBM Information Management System (IMS).

## Network Database

- It uses a graph-like model to allow more complex relationships between entities.
- Unlike the hierarchical model, it permits each child to have multiple parents, enabling many-to-many relationships.
- Data is represented using records and sets, where sets define the relationships.
- It is more flexible than the hierarchical model and better suited for applications with complex data linkages.

****Example:**** Integrated Data Store (IDS), TurboIMAGE.

## Cloud-Based Database

- They are hosted on cloud computing platforms like AWS, Azure or Google Cloud.
- They offer on-demand scalability, high availability, automatic backups and remote accessibility.
- These databases can be relational (SQL) or non-relational (NoSQL) and are maintained by cloud service providers, reducing administrative overhead.
- They support modern application requirements, including distributed access and real-time analytics.
****Example:**** Amazon RDS (for SQL), MongoDB Atlas (for NoSQL), Google BigQuery.

# Database Languages

![[Pasted image 20260123160121.png]]

## DDL 

Data Definition Language, which deals with database schemas and descriptions, of how the data should reside in the database.

- ****CREATE:**** to create a database and its objects like (table, index, views, store procedure, function and triggers)
- ****ALTER:**** alters the structure of the existing database
- ****DROP:**** delete objects from the database
- ****TRUNCATE:**** remove all records from a table, including all spaces allocated for the records are removed
- ****COMMENT:**** add comments to the data dictionary
- ****RENAME:**** rename an object

## DML 

Data Manipulation Language, focuses on manipulating the data stored in the database, enabling users to retrieve, add, update and delete data.

- ****INSERT:**** insert data into a table
- ****UPDATE:**** updates existing data within a table
- ****DELETE:**** Delete all records from a database table
- ****MERGE:**** UPSERT operation (insert or update)
- ****CALL:**** call a PL/SQL or Java subprogram
- ****EXPLAIN PLAN:**** interpretation of the data access path
- ****LOCK TABLE:**** concurrency Control

## DCL

- ****GRANT****: Provides specific privileges to a user (e.g., SELECT, INSERT).
- ****REVOKE****: Removes previously granted permissions from a user.

## TCL (Transaction Control Language)

- ****ROLLBACK****: Undoes changes made during a transaction.
- ****COMMIT****: Saves all changes made during a transaction.
- ****SAVEPOINT****: Sets a point within a transaction to which one can later roll back.

## DQL (Data Query Language)

Its main command is SELECT, which allows users to fetch specific information based on their requirements.

# Role of DBMS

A Data Base Management System is a system software for easy, efficient and reliable data processing and management. It can be used for:

- Managing data efficiently with optimized storage and retrieval.
- Providing simple query languages like SQL.
- Ensuring data consistency and concurrency with transaction controls.
- Enforcing robust security policies with built-in access controls.

Below are the main reason why we need a DBMS software.

## Organizing and Managing Data

A DBMS helps keep data neat and structured, making it easy to find and use. With features like indexing and fast search, you can quickly pull up exactly what you need—even from massive databases.

## Data Security and Privacy

DBMSs keep your data safe with login controls, encryption, and strict access rules. They also help you stay compliant with privacy laws like GDPR and HIPAA.

## Maintaining Accuracy and Consistency

To avoid errors, DBMSs make sure the data stays accurate and consistent. Tools like constraints and transactions ensure updates happen safely and correctly.

## Multiple Users, No Conflict

DBMSs let many people access data at the same time without messing things up. They use smart locking to prevent conflicts or data loss.

## Better Data Insights

With built-in tools for analysis, reporting, and even machine learning, DBMSs help turn raw data into useful insights—making decision-making faster and smarter.

## Grows With Your Needs

As your data grows, a DBMS can scale up—either by adding more servers or boosting current ones. It's flexible too, so you can adapt it as your business evolves.

## Saves Time and Money

DBMSs cut costs by reducing data duplication, automating routine tasks, and simplifying maintenance. Overall, they’re more efficient than old-school file systems.

# Comparison of File System and DBMS

|Feature|File System|DBMS|
|---|---|---|
|Data Redundancy|High|Low|
|Data Security|Minimal|Advanced|
|Relationship Support|None|Full|
|Multi-user Access|Limited|Fully Supported|

# 1-Tier DBMS Architecture

In 1-Tier Architecture, the user works directly with the database on the same system. This means the client, server and database are all in one application. The user can open the application, interact with the data and perform tasks without needing a separate server or network connection.
![[Pasted image 20260123160958.png]]

## Advantages

- ****Simple Architecture:**** 1-Tier Architecture is the most simple architecture to set up, as only a single machine is required to maintain it.
- ****Cost-Effective:**** No additional hardware is required for implementing 1-Tier Architecture, which makes it cost-effective.
- ****Easy to Implement:**** 1-Tier Architecture can be easily deployed and hence it is mostly used in small projects.

## Disadvantages

- ****Limited to Single User:**** Only one person can use the application at a time. It’s not designed for multiple users or teamwork.
- ****Poor Security:**** Since everything is on the same machine, if someone gets access to the system, they can access both the data and the application easily.
- ****No Centralized Control:**** Data is stored locally, so there's no central database. This makes it hard to manage or back up data across multiple devices.
- ****Hard to Share Data:**** Sharing data between users is difficult because everything is stored on one computer.

# 2-Tier DBMS Architecture

The 2-tier architecture is similar to a basic client-server model. The application at the client end directly communicates with the database on the server side. APIs like ODBC and JDBC are used for this interaction. The server side is responsible for providing query processing and transaction management functionalities.
![[Pasted image 20260123161136.png]]

## Advantages

- ****Easy to Access:**** 2-Tier Architecture makes easy access to the database, which makes fast retrieval.
- ****Scalable:**** We can scale the database easily, by adding clients or upgrading hardware.
- ****Low Cost:**** 2-Tier Architecture is cheaper than 3-Tier Architecture and Multi-Tier Architecture.
- ****Easy Deployment:**** 2-Tier Architecture is easier to deploy than 3-Tier Architecture.
- ****Simple:**** 2-Tier Architecture is easily understandable as well as simple because of only two components.
## Disadvantages

- ****Limited Scalability:**** As the number of users increases, the system performance can slow down because the server gets overloaded with too many requests.
- ****Security Issues:**** Clients connect directly to the database, which can make the system more vulnerable to attacks or data leaks.
- ****Tight Coupling:**** The client and the server are closely linked. If the database changes, the client application often needs to be updated too.
- ****Difficult Maintenance:**** Managing updates, fixing bugs, or adding features becomes harder when the number of users or systems increases.

# 3-Tier DBMS Architecture

In 3-Tier Architecture, there is another layer between the client and the server. The client does not directly communicate with the server. Instead, it interacts with an application server which further communicates with the database system and then the query processing and transaction management takes place. This intermediate layer acts as a medium for the exchange of partially processed data between the server and the client. This type of architecture is used in the case of large web applications.
![[Pasted image 20260123180811.png]]

## Advantages

- ****Enhanced scalability:**** Scalability is enhanced due to the distributed deployment of application servers. Now, individual connections need not be made between the client and server.
- ****Data Integrity:**** 3-Tier Architecture maintains Data Integrity. Since there is a middle layer between the client and the server, data corruption can be avoided/removed.
- ****Security:**** 3-Tier Architecture Improves Security. This type of model prevents direct interaction of the client with the server thereby reducing access to unauthorized data.

## Disadvantages

- ****More Complex:**** 3-Tier Architecture is more complex in comparison to 2-Tier Architecture. Communication Points are also doubled in 3-Tier Architecture.
- ****Difficult to Interact:**** It becomes difficult for this sort of interaction to take place due to the presence of middle layers.
- ****Slower Response Time:**** Since the request passes through an extra layer (application server), it may take more time to get a response compared to 2-Tier systems.
- ****Higher Cost:**** Setting up and maintaining three separate layers (client, server and database) requires more hardware, software and skilled people. This makes it more expensive.

# Entity Relationship Model

The Entity-Relationship Model (ER Model) is a conceptual model for designing a database. This model represents the logical structure of a database, including entities, their attributes, and relationships between them.

- ****Entity:**** An object that is stored as data. E.g: __Student__, __Course,__ or __Company__.
- ****Attribute:**** Properties that describe an entity. E.g: __StudentID__, __CourseName__, or __EmployeeEmail__.
- ****Relationship:**** A connection between entities. E.g: __Student__ enrolls in a __Course__.

![[Pasted image 20260124124515.png]]

## Types of Entity

### Strong Entity

A `Strong Entity` is a type of entity that has a key Attribute that can uniquely identify each instance of the entity. A Strong Entity does not depend on any other Entity in the Schema for its identification. It has a primary key that ensures its uniqueness and is represented by a rectangle in an ER diagram.

### Weak Entity

A `Weak Entity` cannot be uniquely identified by its own attributes alone. It depends on a strong entity to be identified. A weak entity is associated with an `identifying entity` (strong entity), which helps in its identification.

## Question: Difference between Weak Entity and Strong Entity:

Strong Entity always has a primary key, while weak entity has partial discriminator key
Strong Entity is not dependent on any other entity, while weak entity is dependent on strong
Strong entity has either total participation or partial participation, weak entity has total participation constraint.

## Types of Attributes

### Key Attributes

The attribute which uniquely identifies each entity in the entity set is called the key attribute. For example, Roll_No will be unique for each student. In ER diagram, the key attribute is represented by an oval with an underline.

### Composite Attribute

An `attribute composed of many other attributes is called a composite attribute`. For example, the Address attribute of the student Entity type consists of Street, City, State, and Country. In ER diagram, the composite attribute is represented by an oval comprising of ovals.

### Multivalued Attribute

An `attribute consisting of more than one value for a given entity.` For example, Phone_No (can be more than one for a given student). In ER diagram, a multivalued attribute is represented by a double oval.

### Derived Attribute

An `attribute that can be derived from other attributes` of the entity type is known as a derived attribute. e.g.; Age (can be derived from DOB). In ER diagram, the derived attribute is represented by a dashed oval.

## Relationship Type and Relationship Set

A Relationship Type represents the association between entity types. For example, ‘Enrolled in’ is a relationship type that exists between entity type Student and Course.

`A set of relationships of the same type is known as a relationship set.`

## Degree of a Relationship Set

`The number of different entity sets participating in a relationship set is called the degree of a relationship set.`

### Unary/Recursive Relationship:
When there is only ONE entity set participating in a relation.
![[Pasted image 20260304111146.png]]

### Binary Relationship
When there are TWO entities set participating in a relationship.
![[Pasted image 20260304113117.png]]

### Ternary Relationship
When there are three entity sets participating in a relationship.
![[Pasted image 20260304113140.png]]

### N-ary Relationship
When there are N entities set participating in a relationship.
![[Pasted image 20260304113213.png|697]]

## Cardinality in ER Model

The maximum number of times an entity of an entity set participates in a relationship set
Can be of different types.

### 1. One-to-one

When each entity in each entity set can take part only once in the relationship, the cardinality is one-to-one.

![[Pasted image 20260514114756.png]]

### 2. One-to-Many

In a one-to-many relationship, one entity can be associated with multiple entities.

![[Pasted image 20260514114835.png]]

### 3. Many-to-One

When entities in one entity set can take part only once in the relationship set and entities in other entity sets can take part more than once in the relationship set, cardinality is many to one.

![[Pasted image 20260514114949.png]]

### 4. Many-to-Many

When entities in all entity sets can take part more than once in the relationship cardinality is many to many.

![[Pasted image 20260514115022.png]]

# Enhanced ER Model

## 1. Super class and Subclass

Super class is a higher-level entity set that has common attributes. Subclass is a lower-level entity set that inherits attributes and relationships from its super class but also has its own specific attributes or relationships. This supports the concept of inheritance, where a subclass automatically possesses the features of the super class.

# Generalisation, Specialisation and Aggregation

## Generalisation

Process of extracting common properties from a set of entities and creating a generalized entity from it. It is a bottom-up approach in which two or more entities can be generalized to a higher-level entity if they have some attributes in common.

![[Pasted image 20260602203335.png]]

## Specialisation

An entity is divided into sub-entities based on its characteristics. It is a top-down approach where the higher-level entity is specialised into two or more lower-level entities.

![[Pasted image 20260602203423.png]]

### Inheritance

It is an important feature of generalization and specialization. In specialization, a higher-level entity is divided into lower-level sub-entities that inherit its attributes. In generalization, similar lower-level entities are combined into a higher-level entity that holds common attributes. In both cases, inheritance allows sub-entities to reuse the properties of the parent entity.

- ****Attribute inheritance:**** It allows lower level entities to inherit the attributes of higher level entities but not vice versa. In below diagram Car is a subclass of Vehicle and inherits its attributes.
- ****Relationship Inheritance****: Sub-entities also inherit relationships of the parent entity.
- ****Overriding Inheritance****: Sub-entities can override or add their own attributes or behaviors different from the parent.
- ****Participation inheritance:**** Participation inheritance in ER modeling refers to the inheritance of participation constraints from a higher-level entity (superclass) to a lower-level entity (subclass). It ensures that subclasses adhere to the same participation rules in relationships, although attributes and relationships themselves are inherited differently.

## Aggregation

- An ER diagram is not capable of representing the relationship between an entity and a relationship which may be required in some scenarios.
- In those cases, a relationship with its corresponding entities is aggregated into a higher-level entity.
- Aggregation is an abstraction through which we can represent relationships as higher-level entity sets.

# File Organisation

`File organization in DBMS refers to the method of storing data records in a file so they can be accessed efficiently. It determines how data is arranged, stored, and retrieved from physical storage.`

## Objective

- It helps in the faster selection of records i.e. it makes the process faster.
- Different Operations like inserting, deleting, and updating different records are faster and easier.
- It prevents us from inserting duplicate records via various operations.
- It helps in storing the records or the data very efficiently at a minimal cost.

## Types

- Sequential File Organization
- Heap File Organization
- Clustered File Organization
- ISAM (Indexed Sequential Access Method)
- Hash File Organization
- B+ Tree File Organization

## Sequential File Organization

File is stored one after another in a sequential manner. There are two ways to implement this method:

### 1. Pile File Method

Records stored in sequence, one after the other in which they are inserted into the tables

![[Pasted image 20260501120050.png]]

**Insertion of new record**: Let the R1, R3, and so on up to R5 and R4 be four records in the sequence. Here, records are nothing but a row in any table. Suppose a new record R2 has to be inserted in the sequence, then it is simply placed at the end of the file.

![[Pasted image 20260501120129.png]]

### 2. Sorted File method

Whenever a new record is inserted, it is always sorted (either ascending or descending). Sorting may be based on `primary key` or any other key.

**Insertion**: Let us assume that there is a preexisting sorted sequence of four records R1, R3, and so on up to R7 and R8. Suppose a new record R2 has to be inserted in the sequence, then it will be inserted at the end of the file and then it will sort the sequence.

![[Pasted image 20260501120313.png]]

### Advantages of Sequential Organization

- Fast and efficient method for huge amounts of data.
- Simple design.
- Files can be easily stored in[magnetic tapes](https://www.geeksforgeeks.org/computer-organization-architecture/magnetic-tape-memory/) i.e. cheaper storage mechanism.

### Disadvantages

- Time wastage as we cannot jump on a particular record that is required, but we have to move in a sequential manner which takes our time.
- The sorted file method is inefficient as it takes time and space for sorting records.

## Heap File Organization

It works with `data blocks`. Records are inserted at the end of the file, into data blocks. No sorting required. If data block is full, new record is stored in some other block. Here the other data block need not be the very next data block, but it can be any block in the memory. It is the responsibility of DBMS to store and manage the new records.

**Insertion**: Suppose we have four records in the heap R1, R5, R6, R4, and R3, and suppose a new record R2 has to be inserted in the heap then, since the last data block i.e data block 3 is full it will be inserted in any of the data blocks selected by the DBMS, let's say data block 1.

![[Pasted image 20260501121317.png]]

If we want to search, delete or update data in the heap file Organization we will traverse the data from the beginning of the file till we get the requested record. Thus if the database is very huge, searching, deleting, or updating the record will take a lot of time.

### Advantages:

- Fetching and retrieving records is faster than sequential records but only in the case of small databases.
- When there is a huge number of data that needs to be loaded into the [database](https://www.geeksforgeeks.org/dbms/what-is-database/) at a time, then this method of file Organization is best suited.

### Disadvantages

- The problem of unused memory blocks.
- Inefficient for larger databases.

# Storage Types

1. Primary Memory
2. Secondary Memory
3. Tertiary Memory

![[Pasted image 20260501121510.png]]

## 1. Primary Memory

Directly accessible by the central processing unit, meaning that it doesn't require any other devices to read from it.

- The size of these devices is considerably smaller and they are volatile. 
- According to performance and speed, the primary memory devices are the fastest devices, and this feature is in direct correlation with their capacity. 
- These primary memory devices are usually more expensive due to their increased speed and performance.

***Cache Memory:*** Cache Memory is a special very high-speed memory. It is used to speed up and synchronize with a high-speed CPU. Cache memory is costlier than main memory or disk memory but more economical than CPU registers. Cache memory is an extremely fast memory type that acts as a buffer between RAM and the CPU.

## 2. Secondary Memory

Devices that can be accessed for storing data that will be needed at a later point in time for various purposes or database actions. Therefore, these types of storage systems are sometimes called backup units as well.

- It is also regarded as a temporary storage system since it can hold data when needed and delete it when the user is done with it.  Compared to primary storage devices as well as tertiary devices, these secondary storage devices are slower with respect to actions and pace. 
- It usually has a higher capacity than primary storage systems

**Flash Memory**: also known as flash storage, is a type of nonvolatile memory that erases data in units called blocks and rewrites data at the byte level. Flash memory is widely used for storage and data transfer in consumer devices, enterprise systems, and industrial applications.

**Magnetic Disk Storage**: type of secondary memory that is a flat disc covered with a magnetic coating to hold information. It is used to store various programs and files. The polarized information in one direction is represented by 1, and vice versa. The direction is indicated by 0.

## 3. Tertiary Memory

devices that can hold a large amount of data without being constantly connected to the server or the peripherals. A device of this type is connected either to a server or to a device where the database is stored from the outside.

- Due to the fact that tertiary storage provides more space than other types of device memory but is most slowly performing, the cost of tertiary storage is lower than primary and secondary. As a means to make a backup of data, this type of storage is commonly used for making copies from servers and databases.
- The ability to use secondary devices and to delete the contents of the tertiary devices is similar.

**Optical Storage**: type of storage where reading and writing are to be performed with the help of a laser. Typically data written on CDs and DVDs

**Tape Storage**: type of storage data where we use magnetic tape to store data. It is used to store data for a long time and also helps in the backup of data in case of data loss.

# Indexing

`Indexing in databases is a data structure technique used to speed up data retrieval operations by minimizing the number of disk accesses required to locate records.`

## Attributes of Indexing

- ****Access Types:**** This refers to the type of access, such as value-based search, range access, etc.
- ****Access Time:**** Refers to the time needed to find a particular data element or set of elements.
- ****Insertion Time:**** To the time taken to find the appropriate space and insert new data.
- ****Deletion Time:**** Time taken to find an item and delete it as well as update the index structure.
- ****Space Overhead:**** It refers to the additional space required by the index.

## File Organisation in Indexing

### 1. Sequential (Ordered) File Organization

Indices are based on a sorted ordering of the values. These are generally fast and a more traditional type of storing mechanism. These Ordered or Sequential file organizations might store the data in a dense or sparse format.

#### Dense Index: 
a dense index maintains one index entry for every search key value present in the data file.

#### Sparse Index
The index record appears only for a few items in the data file. Each item points to a block as shown. To locate a record, we find the index record with the largest search key value less than or equal to the search key value we are looking for.

### Hash File Organization

Uses a hash function to map keys to buckets.

- Offers fast access for exact-match queries.
- Not suitable for range queries.

## Types of Indexing Methods

### 1. Clustered Indexing

stores related records together in the same file, reducing search time and improving performance, especially for join operations. Data is stored in sorted order based on a key (often a non-primary key) to group similar records

![[Pasted image 20260602200341.png]]

### 2. Primary Indexing

indexing technique in which the index is created on the primary key of a data file. The data records are physically stored in sorted order according to the primary key. The primary key uniquely identifies each record, each index entry corresponds to a block of records and contains the primary key value along with a pointer to the first record of that block. It is commonly used with sequential file organization and improves the efficiency of search operations because the data is ordered.

### 3. Non-Clustered or Secondary Indexing

A non-clustered index just tells us where the data lies, i.e. it gives us a list of virtual pointers or references to the location where the data is actually stored. Data is not physically stored in the order of the index. Instead, data is present in leaf nodes.

## Question: Difference between Primary and Secondary Index

A primary index is an index on a set of fields that includes a primary key for the field and is guaranteed not to contain duplicates.

A secondary index is an index that is not a primary index, and may have duplicated. Example: Employee name can be an example. 

The primary index contains the key fields of the table. Primary index is automatically created when table is activated. If a large table is frequently accessed such that it is is not possible to apply primary index sorting, you should use a secondary index.

## Question: Difference between Dense Index and Sparse Index

index size is larger in dense and smaller in sparse.
time taken to locate data in dense is lesser than sparse
there is more overhead for insertions and deletions in dense index
records in dense index need not be clustered, in sparse they need to be clustered
RAM usage is less in dense than sparse
data pointers in dense point to each record. In sparse, pointers point to fewer records

# JOINS

SQL Joins are used to combine data from two or more tables based on a related column.

## Types

based on how rows from two tables are matched and combined

### 1. Inner Join

used to retrieve rows where matching values exist in both tables.It helps in:

- Combining records based on a related column.
- Returning only matching rows from both tables.
- Excluding non-matching data from the result set.
- Ensuring accurate data relationships between tables.

```
select table1.column1, table2.column2, table2.column1 From table1 INNER JOIN table2 ON table1.matching_column = table2.matching_column;
```

### 2. LEFT Join

used to retrieve all rows from the left table and matching rows from the right table.It helps in:

- Returning all records from the left table.
- Showing matching data from the right table.
- Displaying NULL values where no match exists in the right table.
- Performing outer joins, also known as `LEFT OUTER JOIN`.

```
select table1.column1,table1.column2,table2.column1,...
FROM table1
LEFT JOIN table2
ON table1.matching_column = table2.matching_column;
```

![[Pasted image 20260602175228.png]]

### 3. RIGHT Join

used to retrieve all rows from the right table and the matching rows from the left table.It helps in:

- Returning all records from the right-side table.
- Showing matching data from the left-side table.
- Displaying NULL values where no match exists in the left table.
- Performing outer joins, also known as `RIGHT OUTER JOIN`.

```
select table1.column1, table2.column2, table2.column1,...
FROM table1
RIGHT JOIN table2
ON table1.matching_column = table2.matching_column;
```

![[Pasted image 20260602175431.png]]

### 4. FULL Join

used to combine the results of both LEFT JOIN and RIGHT JOIN. It helps in:

- Returning all rows from both tables.
- Showing matching records from each table.
- Displaying NULL values where no match exists in either table.
- Providing complete data from both sides of the join.

```
select table1.column1, table1.column2, table2.column1,...
FROM table1
FULL JOIN table2
ON table1.matching_column = table2.matching_column;
```

![[Pasted image 20260602175544.png]]

## QUESTION: DIFFERENCE BETWEEN LEFT OUTER JOIN AND RIGHT OUTER JOIN

Left Join retrieves all records from the left table and all matching records from the right table

Right Join retrieves all records from the right table and all matching records from the left table

Their keywords are different. `LEFT JOIN` for Left Join. `RIGHT JOIN` for Right Join.

`Syntax for LEFT JOIN`:
```
SELECT columns  
FROM left_table  
LEFT JOIN right_table ON  
join_condition;
```

`Syntax for RIGHT JOIN`:
```
SELECT columns  
FROM left_table  
RIGHT JOIN right_table ON  
join_condition;
```

# Recovery Techniques
### Log Based Recovery

Log-based recovery in DBMS ensures data can be maintained or restored in the event of a system failure. The DBMS records every transaction on stable storage, allowing for easy data recovery when a failure occurs. For each operation performed on the database, a log file is created. Transactions are logged and verified before being applied to the database, ensuring data integrity.

![[Pasted image 20260602180047.png]]

`A log is a sequence of records that document the operations performed during database transactions. Logs are stored in a log file for each transaction, providing a mechanism to recover data in the event of a failure. For every operation executed on the database, a corresponding log record is created. It is critical to store these logs before the actual transaction operations are applied to the database, ensuring data integrity and consistency during recovery processes.`

### Key Operations

#### UNDO

The undo operation reverses the changes made by an uncommitted transaction.  
Even though the transaction did not commit, its dirty pages may have already been written to disk from the buffer.  
Undo restores the old values using the log to remove these partial changes.

#### REDO

The redo operation re-applies the changes of a committed transaction.  
Although the transaction has committed, its updated pages may not have been flushed to disk before the crash.  
Redo ensures that committed changes are safely applied to the database.

## Shadow Paging

Shadow Paging is an alternative recovery technique that avoids the need for a log. It works by keeping two versions of the database pages during a transaction: a current page table and a shadow page table

- The shadow page table points to the original, unmodified database pages from before the transaction began. It's a "shadow" of the consistent database state.
- When a transaction starts modifying data, new copies of the modified pages are created. The current page table is updated to point to these new pages while the shadow page table remains unchanged.

## Question: How is logging different than Shadow Paging

Instead of keeping detailed logs like log-based recovery, shadow paging maintains ****two versions**** of the database’s page table:

### 1. Shadow Page Table

- Represents the ****old, stable state**** of the database (before the transaction begins).
- It ****never changes**** during the transaction.
- If a failure occurs, the system can immediately revert to this version.

### 2. Current Page Table

- A copy of the shadow page table made when a transaction starts.
- All updates during the transaction are made here.
- Only modified pages are replaced with new pages—this is known as ****copy-on-write**** or ****out-of-place updating****.

# Hashing

Hashing in DBMS is a technique to quickly locate a data record in a database irrespective of the size of the database. For larger databases containing thousands and millions of records, the indexing data structure technique becomes very inefficient because searching a specific record through indexing will consume more time.

- ****Hash Table:**** A hash table is an array or data structure and its size is determined by the total volume of data records present in the database. Each memory location in a hash table is called a '****bucket****' or hash indices and stores a data record's exact location and can be accessed through a hash function.
- ****Bucket:**** A bucket is a memory location (index) in the hash table that stores the data record. These buckets generally store a disk block which further stores multiple records. It is also known as the hash index.
- ****Hash Function:**** A hash function is a mathematical equation or algorithm that takes one data record's primary key as input and computes the hash index as output.

## Types

### Static Hashing

In static hashing, the hash function always generates the same bucket's address.
The primary key is used as the input to the hash function and the hash function generates the output as the hash index (bucket's address) which contains the address of the actual data record on the disk block.

#### Properties

- ****Fixed Table Size:**** The number of buckets remains constant.
- ****Simple Hash Function:**** Typically uses a modulo function.
- ****Best for Known Data Size:**** Efficient when the number of records is known and stable.
- ****Inefficient with Dynamic Data:**** As data grows, collisions increase, leading to bucket overflows or skew.

### Chaining

Chaining is a mechanism in which the hash table is implemented using an array of type nodes, where each bucket is of node type and can contain a long chain of linked lists to store the data records. So, even if a hash function generates the same value for any data record it can still be stored in a bucket by adding a new node.

![[Pasted image 20260602212032.png]]

### Open Addressing (Closed Hashing)

this aims to solve the problem of collision by looking out for the next empty slot available which can store data. It uses techniques like ****linear probing****, ****quadratic probing****, ****double hashing,**** etc.

### Dynamic Hashing

Dynamic hashing is also known as [extendible hashing](https://www.geeksforgeeks.org/dbms/extendible-hashing-dynamic-approach-to-dbms/), used to handle database that frequently changes data sets. This method offers us a way to add and remove data buckets on demand dynamically. This way as the number of data records varies, the buckets will also grow and shrink in size periodically whenever a change is made.

#### Properties

- ****Flexible Hash Function:**** Adapts based on data size.
- ****Directories:**** Point to buckets and may grow in size.
- ****Global Depth:**** Number of bits in directory indices.
- ****Bucket Splitting:**** Prevents overflow and ensures balanced distribution.

# B Trees

A B-Tree is a specialized m-way tree designed to optimize data access, especially on disk-based storage systems.

- In a B-Tree of order ****m****, each node can have up to m children and m-1 keys, allowing it to efficiently manage large datasets.
- The value of m is decided based on disk block and key sizes.
- One of the standout features of a B-Tree is its ability to store a significant number of keys within a single node, including large key values. It significantly reduces the tree’s height, hence reducing costly disk operations.
- B Trees allow faster data retrieval and updates, making them an ideal choice for systems requiring efficient and scalable data management. By maintaining a balanced structure at all times,
- B-Trees deliver consistent and efficient performance for critical operations such as search, insertion, and deletion.

![[Pasted image 20260602212512.png]]

## Properties

`B Tree` of order m can be defined as an `m-way` search tree which satisfies the following properties:

1. All leaf nodes of a `B tree` are at the same level, i.e. they have the same depth (height of the tree).
2. The keys of each node of a `B tree` (in case of multiple keys), should be stored in the ascending order.
3. In a `B tree`, all non-leaf nodes (except root node) should have at least `m/2` children.
4. All nodes (except root node) should have at least `m/2 - 1` keys.
5. If the root node is a leaf node (only node in the tree), then it will have no children and will have at least one key. If the root node is a non-leaf node, then it will have at least `2 children` and at least one key.
6. A non-leaf node with n-1 key values should have n non NULL children.

# B+ Tree

A B+ Tree is an advanced data structure used in database systems and file systems to maintain sorted data for fast retrieval, especially from disk. It is an extended version of the B Tree, where all actual data is stored only in the leaf nodes, while internal nodes contain only keys for navigation.

## Components

- Leaf nodes store all the key values and pointers to the actual data.
- Internal nodes store only the keys that guide searches.
- All leaf nodes are linked together, supporting efficient sequential and range queries.

## Features

- ****Balanced****: Auto-adjusts when data is added or removed, keeping search time efficient.
- ****Multi-level****: Has a root, internal nodes, and leaf nodes (which store the data).
- ****Ordered****: Maintains sorted keys, making range queries easy.
- ****High Fan-out****: Each node has many children, keeping the tree short and fast.
- ****Cache-friendly****: Works well with memory caches for better speed.
- ****Disk-efficient****: Ideal for disk storage due to fast data access.

## How it works

- Each internal node is of the form: <P1, K1, P2, K2, ....., Pc-1, Kc-1, Pc> where c <= a and each ****P********i**** ****is a tree pointer (i.e points to another node of the tree)**** and, each ****K********i**** ****is a key-value**** (see diagram-I for reference).
- Every internal node has : K1 < K2 < .... < Kc-1
- For each search field value 'X' in the sub-tree pointed at by Pi, the following condition holds: Ki-1 < X <= Ki, for 1 < I < c and, Ki-1 < X, for i = c (See diagram I for reference)
- Each internal node has at most 'a' tree pointers.
- The root node has, at least two tree pointers, while the other internal nodes have at least a22a​ tree pointers each.
- If an internal node has 'c' pointers, c <= a, then it has 'c - 1' key values.

![[Pasted image 20260602214724.png]]

# Object Oriented DBMS

The **ODBMS** which is an abbreviation for **object-oriented database management system** is the data model in which data is stored in form of objects, which are instances of classes. These classes and objects together make an object-oriented data model.

## Components

The OODBMS is based on three major components, namely: Object structure, Object classes, and Object identity. These are explained below.

### Object Structure:

The structure of an object refers to the properties that an object is made up of. These properties of an object are referred to as an attribute. Thus, an object is a real-world entity with certain attributes that makes up the object structure. Also, an object encapsulates the data code into a single unit which in turn provides data abstraction by hiding the implementation details from the user.

The object structure is further composed of three types of components: Messages, Methods, and Variables. These are explained below.  

1. **Messages -**   
    A message provides an interface or acts as a communication medium between an object and the outside world. A message can be of two types:   
      
    - **Read-only message:** If the invoked method does not change the value of a variable, then the invoking message is said to be a read-only message. 
    - **Update message:** If the invoked method changes the value of a variable, then the invoking message is said to be an update message.   
         
2. **Methods -**   
    When a message is passed then the body of code that is executed is known as a method. Whenever a method is executed, it returns a value as output. A method can be of two types:   
      
    - **Read-only method:** When the value of a variable is not affected by a method, then it is known as the read-only method. 
    - **Update-method:** When the value of a variable change by a method, then it is known as an update method.   
         
3. **Variables -**   
    It stores the data of an object. The data stored in the variables makes the object distinguishable from one another.

### Object Classes

An object which is a real-world entity is an instance of a class. Hence first we need to define a class and then the objects are made which differ in the values they store but share the same class definition. The objects in turn correspond to various messages and variables stored in them.

```
class CLERK

  { //variables
     char name;
     string address;
     int id;
     int salary;

    //Messages
     char get_name();
     string get_address();
     int annual_salary();
  };
```

An OODBMS also supports inheritance in an extensive manner as in a database there may be many classes with similar methods, variables and messages. Thus, the concept of the class hierarchy is maintained to depict the similarities among various classes.   
  
The concept of encapsulation that is the data or information hiding is also supported by an object-oriented data model. And this data model also provides the facility of abstract data types apart from the built-in data types like char, int, float. ADT's are the user-defined data types that hold the values within them and can also have methods attached to them.   
  
Thus, OODBMS provides numerous facilities to its users, both built-in and user-defined. It incorporates the properties of an object-oriented data model with a database management system, and supports the concept of programming paradigms like classes and objects along with the support for other concepts like encapsulation, inheritance, and the user-defined ADT's (abstract data types).  
 

ODBMS stands for Object-Oriented Database Management System, which is a type of database management system that is designed to store and manage object-oriented data. Object-oriented data is data that is represented using objects, which encapsulate data and behavior into a single entity.

An ODBMS stores and manages data as objects, and provides mechanisms for querying, manipulating, and retrieving the data. In an ODBMS, the data is typically stored in the form of classes and objects, which can be related to each other using inheritance and association relationships.

In an ODBMS, the data is managed using an object-oriented programming language or a specialized query language designed for object-oriented databases. Some of the popular object-oriented database languages include Smalltalk, Java, and C++. Some ODBMS also support standard SQL for querying the data.

ODBMS have several advantages over traditional relational databases. One of the main advantages is that they provide a natural way to represent complex data structures and relationships. Since the data is represented using objects, it can be easier to model real-world entities in the database. Additionally, ODBMS can provide better performance and scalability for applications that require a large number of small, complex transactions.

However, there are also some disadvantages to using an ODBMS. One of the main disadvantages is that they can be more complex and harder to use than traditional relational databases. Additionally, ODBMS may not be as widely used and supported as traditional relational databases, which can make it harder to find expertise and support. Finally, some applications may not require the advanced features and performance provided by an ODBMS, and may be better suited for a simpler database solution

## Features:

**Object-oriented data model:** ODBMS uses an object-oriented data model to store and manage data. This allows developers to work with data in a more natural way, as objects are similar to the objects in the programming language they are using.

**Complex data types:** ODBMS supports complex data types such as arrays, lists, sets, and graphs, allowing developers to store and manage complex data structures in the database.

**Automatic schema management:** ODBMS automatically manages the schema of the database, as the schema is defined by the classes and objects in the application code. This eliminates the need for a separate schema definition language and simplifies the development process.

**High performance:** ODBMS can provide high performance, especially for applications that require complex data access patterns, as objects can be retrieved with a single query.

**Data integrity:** ODBMS provides strong data integrity, as the relationships between objects are maintained by the database. This ensures that data remains consistent and correct, even in complex applications.

**Concurrency control:** ODBMS provides concurrency control mechanisms that ensure that multiple users can access and modify the same data without conflicts.

**Scalability:** ODBMS can scale horizontally by adding more servers to the database cluster, allowing it to handle large volumes of data.

**Support for transactions:** ODBMS supports transactions, which ensure that multiple operations on the database are atomic and consistent.

## Advantages

**Supports Complex Data Structures:** ODBMS is designed to handle complex data structures, such as inheritance, polymorphism, and encapsulation. This makes it easier to work with complex data models in an object-oriented programming environment.

**Improved Performance:** ODBMS provides improved performance compared to traditional relational databases for complex data models. ODBMS can reduce the amount of mapping and translation required between the programming language and the database, which can improve performance.

**Reduced Development Time:** ODBMS can reduce development time since it eliminates the need to map objects to tables and allows developers to work directly with objects in the database.

**Supports Rich Data Types:** ODBMS supports rich data types, such as audio, video, images, and spatial data, which can be challenging to store and retrieve in traditional relational databases.

**Scalability:** ODBMS can scale horizontally and vertically, which means it can handle larger volumes of data and can support more users.

# Distributed DBMS

A Distributed Database System (DDBS) is a collection of multiple databases spread across different physical locations, connected via a network.

- Data is stored across multiple sites but appears as a single database to users.
- Enables local access to data, improving response time and performance.
- Supports parallel processing, allowing multiple operations at the same time.

![[Pasted image 20260602215419.png]]

## Homogenous DB

In a homogeneous database, all different sites store database identically. The operating system, database management system, and the data structures used all are the same at all sites.

****Features****:

- Unified query language and interface.
- Low integration complexity.
- Efficient synchronization.

****Example:**** A bank with branches in different cities uses Oracle DB at every location. All databases have the same structure and are synchronized regularly.

## Heterogenous DB

In a heterogeneous distributed database, different sites may use different DBMSs****,**** schemas, or data models, making query processing and transactions difficult. Some sites may not even be aware of others, so translation mechanisms are needed for communication. 

****Features:****

- Supports interoperability between diverse systems.
- Complex query optimization and transaction management.
- Useful in mergers or collaborations between organizations.

****Example****: A logistics company uses MySQL for inventory, MongoDB for vehicle tracking, and PostgreSQL for billing. Integration middleware allows unified querying across these platforms.

## Client-Server Distributed DB

The server stores and manages the database, while clients send queries over the network. It offers centralized control with distributed access, making it ideal for enterprise systems and web applications. Clients can be lightweight while the server handles heavy processing. Example: Web application interacting with a central PostgreSQL server.

****Features:****

- Simplifies resource management.
- Central servers can be optimized for performance.
- Easily scalable with more clients.

****Example****: An e-commerce website where the frontend (client) is hosted separately and interacts with a central PostgreSQL server to manage orders, users, and inventory.

## Peer-to-Peer Distributed DB

All nodes are equal, with no fixed client or server roles. Each node can store data and also process queries, leading to decentralized control. It supports fault tolerance and high availability. Example: Blockchain networks like Ethereum, where each node maintains a part of the distributed ledger.

****Features:****

- No single point of failure.
- Useful in decentralized and distributed apps.
- High availability and data redundancy.

****Example:**** Blockchain-based databases like Ethereum or BitTorrent-based systems, where each peer maintains part of the ledger and participates equally in transactions.

## Cloud-Based Distributed DB

These systems are deployed on cloud platforms and span multiple geographic regions for scalability and reliability. They abstract infrastructure details and are offered as ****DBaaS, making them ideal for dynamic workloads. Example: Google Cloud Spanner and Amazon DynamoDB used for global applications.****

****Features:****

- Automatic scaling and replication.
- Pay-as-you-use pricing.
- Global availability and disaster recovery.

### Example:

- ****Google Cloud Spanner****: Global-scale relational database.
- ****Amazon DynamoDB:**** Key-value and document database with high performance.
- ****Azure Cosmos DB****: Multi-model, globally distributed DBMS.

# Components and Features of Distributed DB

## Replication

In replication, copies of the same data are stored at two or more sites. If every site has the full database, it's called full replication. This improves data availability and allows faster, parallel query processing. However, updates must be made at all sites, or data may become inconsistent. It also adds overhead and makes concurrency control more complex.

## Fragmentation

The relations are fragmented (i.e., they're divided into smaller parts) and each of the fragments is stored in different sites where they're required. It must be made sure that the fragments are such that they can be used to reconstruct the original relation (i.e, there isn't any loss of data).   
Fragmentation is advantageous as it doesn't create copies of data, consistency is not a problem.   
Fragmentation of relations can be done in two ways: 

- ****Horizontal fragmentation - Splitting by rows****  
    The relation is fragmented into groups of tuples so that each tuple is assigned to at least one fragment.
- ****Vertical fragmentation - Splitting by columns****  
    The schema of the relation is divided into smaller schemas. Each fragment must contain a common candidate key so as to ensure a lossless join.

## Concurrency Control

Concurrency control ensures data remains accurate when multiple transactions run at the same time. Without it, issues like lost updates or dirty reads can occur. Its goal is to make parallel transactions behave as if run one by one. Common methods include locking, timestamps, and optimistic concurrency.

# Normalisation

Normalization is an important process in database design that helps improve the database's efficiency, consistency, and accuracy. It makes it easier to manage and maintain the data and ensures that the database is adaptable to changing business needs.

- It is the process of organizing the attributes of the database to reduce or eliminate data redundancy (having the same data in different places), which otherwise unnecessarily increases the size of the database.
- Data redundancy leads to inconsistency problems during insert, delete, and update operations, making data management difficult.
- Normalization involves splitting a table into multiple tables, which must be linked each time a query requires data from the split tables.

# ACID Properties

![[Pasted image 20260602224031.png]]

## Atomicity

Atomicity means a transaction is all-or-nothing either all its operations succeed, or none are applied. If any part fails, the entire transaction is rolled back to keep the database consistent.

- ****Commit****: If the transaction is successful, the changes are permanently applied.
- ****Abort/Rollback****: If the transaction fails, any changes made during the transaction are discarded.

## Consistency

Consistency in transactions means that the database must remain in a valid state before and after a transaction.

- A valid state follows all defined rules, constraints, and relationships (like primary keys, foreign keys, etc.).
- If a transaction violates any of these rules, it is rolled back to prevent corrupt or invalid data.
- If a transaction deducts money from one account but doesn't add it to another (in a transfer), it violates consistency.

## Isolation

Isolation ensures that transactions run independently without affecting each other. Changes made by one transaction are not visible to others until they are committed.

It ensures that the result of concurrent transactions is the same as if they were run one after another, preventing issues like:

- ****Dirty reads:**** reading uncommitted data
- ****Non-repeatable reads:**** data changes between two reads
- ****Phantom reads:**** new rows appear during a transaction

## Durability

Durability ensures that once a transaction is committed, its changes are permanently saved, even if the system fails. The data is stored in non-volatile memory, so the database can recover to its last committed state without losing data.

## How ACID Properties Impact DBMS Design and Operation

The ACID properties, in totality, provide a mechanism to ensure the correctness and consistency of a database in a way such that each transaction is a group of operations that acts as a single unit, produces consistent results, acts in isolation from other operations, and updates that it makes are durably stored.

### 1. Data Integrity and Consistency

ACID properties safeguard the data integrity of a DBMS by ensuring that transactions either complete successfully or leave no trace if interrupted. They prevent partial updates from corrupting the data and ensure that the database transitions only between valid states.

### 2. Concurrency Control

ACID properties provide a solid framework for managing concurrent transactions. Isolation ensures that transactions do not interfere with each other, preventing data anomalies such as lost updates, temporary inconsistency, and uncommitted data.

### 3. Recovery and Fault Tolerance

Durability ensures that even if a system crashes, the database can recover to a consistent state. Thanks to the Atomicity and Durability properties, if a transaction fails midway, the database remains in a consistent state.

# Transaction States

![[Pasted image 20260602225136.png]]

### 1. Active State

- It is the first stage of any [transaction](https://www.geeksforgeeks.org/dbms/transaction-in-dbms/) when it has begun to execute. The execution of the transaction takes place in this state.
- Operations such as insertion, deletion, or updation are performed during this state.
- During this state, the data records are under manipulation and they are not saved to the database, rather they remain somewhere in a buffer in the main memory.

### 2. Partially Committed

- The transaction has finished its final operation, but the changes are still not saved to the database.
- After completing all read and write operations, the modifications are initially stored in main memory or a local buffer. If the changes are made permanent in the database then the state will change to "committed state" and in case of failure it will go to the "failed state". 

### 3. Committed

This state of transaction is achieved when all the transaction-related operations have been executed successfully along with the Commit operation, i.e. data is saved into the database after the required manipulations in this state. This marks the successful completion of a transaction.

### 4. Failed State

If any of the transaction-related operations cause an error during the active or partially committed state, further execution of the transaction is stopped and it is brought into a failed state. Here, the database recovery system makes sure that the database is in a consistent state.

### 5. Aborted State

If a transaction reaches the failed state due to a failed check, the database recovery system will attempt to restore it to a consistent state. If recovery is not possible, the transaction is either rolled back or cancelled to ensure the database remains consistent.

In the aborted state, the DBMS recovery system performs one of two actions:

- ****Kill the transaction****: The system terminates the transaction to prevent it from affecting other operations.
- ****Restart the transaction****: After making necessary adjustments, the system reverts the transaction to an active state and attempts to continue its execution.

### 6. Terminated State

It refers to the final state of a transaction, indicating that it has completed its execution. Once a transaction reaches this state, it has either been successfully committed or aborted. In this state, no further actions are required from the transaction, as the database is now stable.

# Schedules

![[Pasted image 20260602225221.png]]

## Serial Schedule

Schedules in which transactions execute one after another without interleaving., i.e., no transaction starts until a running transaction has ended are called serial schedules.

- Only one transaction runs at a time.
- No concurrency issues.
- Simple and always consistent.

## Non-serial Schedule

Transactions execute in an interleaved manner in Non-Serial Schedule.

- Allows concurrency.
- Must be checked for correctness.
- May or may not be serializable.

Non-serial schedules are of two types:

### Serilizable Schedule

A non-serial schedule that behaves like a serial schedule.

# Serializability

In DBMS, serializability ensures the database remains correct even when multiple transactions run at the same time. It makes transactions behave as if they were executed one after another in some order. This prevents conflicts, maintains data consistency, and ensures reliable results, just like in a single, orderly sequence of transactions.

## Types

![[Pasted image 20260602225426.png]]

### Conflict 

Conflict serializability ensures database consistency by checking if a non-serial schedule can be rearranged into a serial order by swapping non-conflicting operations. It prevents conflicting operations (like read/write on the same data) from executing at the same time.

### View

View serializability ensures that a non-serial schedule results in the same final outcome as a serial schedule, maintaining database consistency.

To further understand view serializability in DBMS, we need to understand the schedules S1 and S2. The two transactions T1 and T2 should be used to establish these two schedules. Each schedule must follow the three transactions in order to retain the equivalent of the transaction. These three circumstances are listed below.

1. ****Same Transactions****: Both schedules must include the same set of transactions.
2. ****Same Writes****: Each data item must be written by the same transaction in both schedules.
3. ****Same Reads****: Each read must read the same value (from the same write) in both schedules.




