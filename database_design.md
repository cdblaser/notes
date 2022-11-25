# DATABASE DESIGN COURSE - FREECODECAMP - BY CALEB CURRY

# Notes

## Definitions

1. Data - anything in our tables
2. Database - what stores our data
3. Relational database- stores things in tables
4. DBMS - manage database with code (create database/tables)
5. RDBMS - manage tables and values in tables (manipulate values)
6. Null - no data in field
7. Abomalies - errors in our data/data integrity
8. Entity Integrity - uniqueness in table
9. Referential Integrity - maintains integrity across multiple linked tables
10. Domain Integrity - data in tables has integrity
11. Entity - anything we store data about (e.g. person)
12. Attributes - anything we store about the entity
13. Relation - relationship between two sets of data, ie a table
14. Tuple - a row in the table
15. Table - physical representation of relation
16. Rows and columns - rows are horizontal and are relations, column is each attribute
17. File - a table
18. Record - a row
19. Field - a column
20. Entry - a row
21. Database Design - process of designing your tables to have tables and integrity
22. Schema - Conceptual, Logical, Physical
23. Normalization - steps we follow to get best database design
24. Naming Convention - consistency across developers
25. Keys - make everything unique in a table
26. SQL, DDL, DML - define data, manipulate data
27. SQL Keywords - lets you do stuff with tables
28. Frontend, backend, client, server - we already know this!
29. Serverside scripting language - how to communicate with server
30. Views - views differ based on user level, page, etc. e.g. regular user and admin have different views
31. Joins - connects two tables together

## Relational Databases

- Entity - what we're storing data about
- Attribute - the data we store for the entity
  e.g. Entity = person, Attributes = username, password, etc.

Try to store data according to the entity.

## Data Integrity

Important for making sure if you have two tables, and for some reason a link is broken between them, that both still update.

Schemas-
Conceptual
Logical
Physical

### Entity Integrity

- Essentially a unique entity. Has a unique ID/key to differentiate

### Referential Integrity

- Two tables with a relationship need to make sure that any changes to one table affect the other table
- Foreign key constraints - e.g. user table and comments table (user is parent, comments is child), if user is deleted, then their comments are deleted

### Domain Integrity

- Basically the range of what we're storing in the database
  e.g. Phone field - will be only integers and 10 digits long

## Foreign keys

- similar to parent-child relationship. It is simply a key that points to another table.

## Data types

- Basically three types:

1. Integer
2. Text - CHAR(20) - allows for text with 20 characters long
3. Date

## Atomic Values

- Values store 1 thing. e.g. a 'name' column that contains full name, Curtis Blaser, is not atomic. Split up the 'name' column into two separate columns, first and last name.

## Relationships

- two tables that are related in some way. e.g. two tables have a similar column such as 'first name' that can be used to relate those tables in some way

### One-to-one Relationships

- e.g. a marriage. One husband to one wife, and that's it. One entity limited to one entity. e.g. each person in the US gets a SSN

### One-to-many Relationships

- one entity can have a relationship with multiple entities, but one of those multiple entities can only be linked to that singular entity. e.g. User table and Comments table, user can make multiple comments, but those comments are only linked to one user.

### Many-to-many relationships

- e.g. polygamous marriage - multiple men can be married to multiple women, and vice versa.
- e.g. Classes and Students tables - students can take multiple classes, and classes can have multiple students.
- Allegedly, many-to-many relationships _DO NOT WORK_ in relational databases.

## Designing one-to-one relationships

- Can be hard depending on situation.
- e.g. Cardholder and card
  Cardholder table - ID, FN, LN, card #, card issue date. But the card issue date relies on the card #
  Card table - ID, card #, issue date, late fee, max.

So instead, let's replace all the card info under the cardholder table, with just a card ID. Card ID can then be related to Card Table by just the ID.

In fact, you don't want to store all the data in one table anyways. Since then the table would have to be called 'user and card table'

## Designing one-to-many relationships

- take the previous example of cardholder with card
- e.g. user table (userId and cardId) and card table (cardId and userId)
- now take that example and say one cardholder can have multiple cards
- e.g. user table can no longer just have cardId, since there are multiple cardIds. Now, it's better to just store the userId in the card table.
- takeaway is to have the 'multiple' table pointing back towards the 'unique' table using a foreign key

## Parent-child relationships

- one table is always parent, one table is always child
- every table has a unique id
- primary key is the parent
- foreign key is the child
- child table inherits properties from parent table
- e.g. multiple card tables all point towards cardholder table
- all childrens must have a parent to point to. parents don't have to point to anything
- when designing which table is parent/child, it is VERY important to always put foreign keys in child
- the reason why many-to-many relationships don't really work. it is hard to decide which table gets primary and which gets foreign key

## Designing many-to-many relationships

- impossible!
- try e.g. multiple classes and multiple students example
- he points out that having null values is bad in sql
- so if you create a student table with 6 class columns, but some students only take 1 class, then you're wasting 5 columns
- there's no real good way to designate which table is the parent and which is the child
- the best way to handle many-to-many relationships is to break it up into MANY one-to-many relationships
- use an intermediary table (also called a junction table) to handle this
- e.g. class table, intermediary table, student table
- looks like 1 --> many, many <--1
- call the intermediary table 'class_students'
  |class| --> |class_students| <-- |students|
- class table has classIds (unique)
- student table has studentIds (unique)
- class_students table will then have two columns, class_id and student_id
- using this intermediary table, we can then associate each student with each class that they're enrolled in, without having a bunch of null values or confusion over parent/child.
- for the above example, it is now two parent tables and one child table (the intermediary)
- when two parent tables are connected to one child table, that is known as a binary table

## Keys

- A key is used to keep things unique. A key is always unique
- e.g. for a user table, username or email could be used as a key since each username and email should be unique.
- in the above example, an email would be a 'natural' key
- keys can be combinations of two columns
- keys should NEVER change! every other column points back to the key. If it changes, it will change the integrity of our database. If we have a reference to the key, then the references have to be updated (which is bad)
- keys should NEVER be null!

## Primary Key Index

- What is an index? Essentially points to data (like an array index)
- Keys are essentially like an index

## Look up tables

- look up tables are basically tables that contain all the possible options/values for another table column.
- e.g. members table (name, membership type) and membership table (key/id, membership type). So if a member is named John and has a Gold membership, you don't have to type 'Gold' for the membership type every time. You can just reference it to the key of '3'.
- look up tables are VERY useful if you need to change a value. that way, instead of changing 1000s of data points individually, you simply change the value that they refer to

## Foreign Key Constraint

- related to look up tables - foreign key constraints protect the integrity of our database by protecting the keys. We don't have to worry about only some values updating.
- Keeps everything unique as well
- Improves speed as well
- Makes updating easier
- Allows for added complexity - e.g. in above example, you can now add more information for each type of membership (gold threshold is $85, or gold membership lasts 3 months)

## Superkey and Candidate key

- Besides the two most types of keys, primary and foreign key, there are two other types of keys. Superkey and Candidate key
- A superkey is any # of columns that forces a row to be unique
- e.g. user table (username, email, pw, fn, mn, ln, bd). Every field is required (no null/blank values)
- how do you know that EVERY row is truly unique?
- Superkeys are basically ANY # of columns that forces a row to be unique. So in above example, username and email are forced to be unique. All other values don't have to be unique.
- Superkeys are usually not enforced/defined in a database.
- A Candidate key is the LEAST # columns needed to enforce uniqueness. So in above example, either the username OR the email could be used to enforce uniqueness.
- when coding, you will never define a Superkey or a Candidate key. It's more a thought process of database design
- Ask yourself, can every row be unique? How many columns are needed? Figure out the least # of columns needed to enforce uniqueness (can use keyword UNIQUE). Once you figure out least # columns needed for uniqueness, that is called the Candidate key.
- You can have more than one Candidate key.
- e.g. Username and email can both be unique. So candidate key #1 is username, #2 is email.
- other candidate keys - could make fn + ln + mn + bd + address - chances of duplicates are EXTREMELY low (but still possible).
- Once you define your Candidate keys, you can then define your Primary key from one of those Candidate keys.

## Primary key

- UNIQUE
- NEVER CHANGE
- NEVER NULL
- are username or email good for this? maybe, it depends on whether you let users change their un/email.

## Alternate key

- essentially a column with unique values that isn't the primary key
- e.g. user table(username, email, fn, ln) - the username can be primary key, email can be alternate key.
- you want to index the alternate key as well

## Surrogate key and Natural key

- these are categories of Primary keys. It's not defined in a database, it's more for database design
- Natural key - something that's naturally in the table. e.g. user table will naturally have email in it.
- Surrogate key - if no natural keys can be designated, then use a madeup column with unique values
- Surrogate keys are private to database
- Surrogate keys are not used to make connections with other tables/dbs as a foreign key

## Surrogate keys vs Natural keys - which should I use?

- Natural keys

  Pros:

1. don't have to add a column
2. They have real world value

   Cons:

3. Sometimes you just won't have a natural key
4. Sometimes you have to decide on the natural key

- Surrogate keys

  Pros:

1. They're always numbers. Sometimes words can be difficult to work with (like username)

Cons:

1. Have to create another column of values with no real world value

- Caleb prefers surrogate keys. They're very simple and easy to use.

## Foreign Keys

- What is a foreign key? it simply references a primary key
- e.g. Class table (class_id, instructor_id, building_id). Which is primary key? Class_id is since all classes have their own id
- What is foreign key? Instructor_id since it links to the instructor table (instructor_id, name)
- Another foreign key? building_id since it links to building table (building_id, location)
- Column rule - rules for your columns. e.g. every value in a column must reference an id from another table
- keep in mind - every table has only one primary key. every table CAN have multiple foreign keys, i.e. multiple references to multiple other tables

## NOT NULL Foreign Keys

- database will NOT accept a NULL value. it will be required
- Cardinality - whether or not a relationship is required
- Primary keys are always required, all the time. Always unique, never null, never change.
- Foreign key values can change, because references can change. e.g. car table (car_id) and owner table (owner_id, car_id) - the owner can sell and buy a car, so his car_id will change)
- Foreign key connections are not always required. e.g. an owner could not own a car yet
- Foreign keys MUST be of the same data type (int, char, etc.) as the reference
- However, foreign keys MAY be null. since they're not required like a primary key
- you can use the keywords NOT NULL to enforce a value for a not-required column (thus making it required basically)

## Foreign Key Constraints

- Foreign keys reference the primary key of another table
- Foreign keys enforce referential integrity
- Keywords can help enforce referential integrity. They will update parent tables if the child table is updated
- Keywords for foreign key constraints. These Foreign key constraints refer to the parent:

1. ON DELETE
2. ON UPDATE

- Keywords for foreign key constraints. These foreign key constraints refer to the children:

1. RESTRICT (no action)
2. CASCADE
3. SET NULL

- Above keywords can combine
- e.g. ON DELETE RESTRICT: This prevents the Parent from being deleted. If FK = 100, and PK = 100, and you try to delete PK, it will error out.
- e.g. ON UPDATE RESTRICT: this prevents the parent from being updated. If FK = 100, and PK = 100, and you try to update PK, it will error out.
- e.g. ON DELETE CASCADE: this causes any deletes that happen to parent to also happen to child. IF FK & PK = 100, and you delete PK, it will delete the FK (entire row) as well.
- e.g. ON UPDATE CASCADE: this causes any updates that happen to parent to also happen to children as well.
- e.g. ON DELETE SET NULL: if PK is deleted, then the FK is set to null

- SET NULL - if you try to use SET NULL with 'NOT NULL', it will error out and revert to previous

- e.g. three tables - parent table (users table), two child tables (comments and purchases tables). Foreign keys point back to Primary keys.

## SIMPLE, COMPOSITE, and COMPOUND keys

- SIMPLE key is just one column to form NATURAL key
- COMPOSITE key is multiple columns combined to form NATURAL key (e.g. fn + ln + email)
- SIMPLE and COMPOSITE do not relate to SURROGATE keys because SURROGATE keys are always one column
- SIMPLE key e.g. username column
- COMPOUND key is when a key is multiple columns AND they are all keys themselves
- Most common example of COMPOUND keys are intermediary tables
- COMPOUND key e.g. Primary tables (user, video) and intermediary (video_comment). video_comment table has user_id and video_id, and each has to be unique.

## Review of Keys

- Superkey --> Candidate Key --> Primary key --> Alternate key
- Foreign key
- Most important keys are Primary Key and Foreign Key
- Primary + Foreign Keys --> Surrogate & Natural keys
- Simple, Composite, & Compound keys
- Constraints --> e.g. FK Constraint ON UPDATE blah blah

## Intro to Entity Relationship Modeling (EER/ERD/ER Model)

- Methods to draw out entire database structure
- Steps:

1. Draw a rectangle as your table
2. Give your table a name
3. Put in columns and column names (he puts them in horizontally like rows)
4. Create any more tables you might need
5. Connect the tables with lines and arrows (arrow points towards Parent)
6. You can put indexes/index types on bottom. And draw relationships via the lines

## Cardinality

- Relationship type of one table to another table
- There are really only two ways to draw connections between tables: 1:1 and 1:N (1 to many)
- M:N (many to many) does not exist, because intermediary tables will be drawn as well. which are technically two separate 1:N connections
- to draw a 1:N connection, it is +----<- (the + is the 1 side, <- is many side). Looks like a cross and a crow's foot
- can also have a circle, he will explain that soon

## Modality

- basically, it's whether a child is required
- when drawing cardinality between two tables, you can add a '0' or '1' to the child side
- a 0 means it does not have the NOT NULL characteristic, meaning it can be null
- a 1 means NOT NULL

## Introduction to Database Normalization

- A process where we go through our database design and correct any problems
- Normal forms are a checklist that we go through and make sure we're following them
- 1NF, 2NF, 3NF (1st normal form, etc.)
- systematic way to produce a good database. Data is atomic, and depends on other data.
- 3NF depends on 2NF, which depends on 1NF. Always start with 1NF

### Dependency

- When one field relies on another field

## 1NF

- below examples are issues:
- e.g. address field - not atomic. need a city, state, zipcode fields separate
- e.g. email field with two emails - not atomic
- e.g. user_id table with two identical rows - PK is not unique
- What if we want to fix example 2? If a user can have two separate emails associated with their account, then pull out the email field from the parent table. Make a child email table and give each email a unique ID as well as linked to the user_id

## 2NF

- What if a column depends on only part of a primary key?
- in order for a column to depend on part of a primary key, then the primary key has to be a composite key (multiple columns)
- how to fix this:
- e.g. author primary table (author_id), book primary table (book_id), and book_author intermediary table. If we want to add isbn to the intermediary table, that would be incorrect because it only depends on the book_id and not the author_id. So instead, add it to the book primary table
- try to remove all partial dependencies

## 3NF

- this is when a column depends on a column which depends on a column
- e.g. review table (review_id, review, star, star_meaning). star_meaning depends on star, and star depends on review_id. so this setup is very bad.
- to fix above example, create a star table (star_id, star, star_meaning). Add star_id to review table. However, now your star table has star and star_meaning, and star_meaning depends on star. So maybe make a star_meaning table and only have 5 entries: 1-5 with their meanings. Then, reference the star_rating.

## Indexes (Clustered, Nonclustered, Composite Index)

- Indexes are basically just like a book index. It tells you exactly where to find certain data
- Nonclustered - data is not actually where index is, index is a separate thing, and it basically just sorts the data
- Clustered index - like a phonebook, it actually organizes the data
- you can only have one clustered index, whereas you can have multiple nonclustered indexes
- clustered indexes are faster and better for searching data, but you can only have one
- nonclustered indexes are like a book index, but the problem with nonclustered indexes is they have to be updated if the data changes (like if you update a book and add more changes, all the indexes for all subsequent pages have to be changed as well)
- PK is usually the clustered index
- indexes also increase the speed of joins. The two columns that you're joining should be indexed to speed up searches
- expect to used composite indexes by themselves, unless you set it up to optimize multiple queries

## Data Types

- 3 main categories of data types:

1. Date - stores like this 02:22:1998 23:22:15. Depends on DBMS

- Timestamps will update when you update a column

2. Numeric - decimal(10) and binary(2). Floating point is stored as binary and can be displayed as decimal. There is also unsigned, signed, etc.

- Binary has problems with math (so does decimal, but decimal is better). e.g. it is impossible to store the value 0.1 in binary

3. String (Var, Varchar(x) where x = # characters/digits). Var(8) means that there will be 8 characters always, so if you input 'blunt' as a value, there will be 3 empty spaces. Varchar(8) is variable meaning size can actually change depending on value.

## Joins

- Joins take a mess and turns it into something beautiful (takes multiple tables and turns them into one table). Joins are a part of DML (data manipulation language)
- e.g. user table --- user_comment table --- comment table. user_comment table will have user_id and comment_id columns, but if you're dipslaying the comment with the username in the front-end, you can't just pull from the user_comment table. You have to pull the username from the user table. So you will use a join
- even if you're the database designer, you need to consider your database design around joins

### Inner Join

- e.g. customer table (customer_id, fn, ln) and card table (card_id, customer_id, limit, amount_paid, amount_owed). Now make a card_customer table using an inner join (fn, ln, amount_owed).
- venn diagram - customers with card (or cards with customers) are the middle of the diagram.
- SELECT fn, ln, amount_owed FROM customer INNER JOIN card ON customer.customer_id = card.customer_id

### Inner Join across 3 tables

- first join two tables, then join that result table with the 3rd table
- e.g. customer table (customer_id, fn, ln), card table (card_id, customer_id, card_type_id, max_amount), card_type table (card_type_id, card_type).
- we want one table with fn, ln, max_amount, and card_type
- first exclusion condition will be customers with no cards, cards with no customers, cards with no card_types, and card_types with no cards
- e.g. user table --- comment table --- video table
- user.user_id = comment.user_id, comment.video_id = video.video_id
- comment.video_id and comment.user_id will be not null
- Caleb talks A LOT about NOT NULL conditions - whether values will be excluded or not because of being NULL. this is a VERY important concept to think about, since you will need to consider what will be included in your INNER JOIN table
- for above example, the INNER JOIN table will return ALL comments, but NOT all users and NOT all videos
- SELECT username, title, comment FROM user INNER JOIN comment ON user.user_id = comment.user_id INNER JOIN video ON video.video_id = comment.video_id

## Introduction to Outer Joins

- JOIN condition from INNER JOIN example: customer.customer_id = card.customer_id
- Three types of Outer Joins: LEFT, RIGHT, and FULL

1. LEFT JOIN - ALL rows from left table will be returned. On the right table, it will only return customer_ids that match from the left table
2. RIGHT JOIN - ALL rows from right table will be returned. On the left table, it will only return customer_ids that much from the right table
3. FULL JOIN - not covered in this video

- LEFT and RIGHT JOINs can be similar if you have NOT NULL declared for certain columns
- LEFT vs RIGHT - depends on your query - SELECT username FROM user INNER JOIN comment - user table is left, comment table is right

## JOIN with NOT NULL columns

- e.g. user table and comment table (user_id NOT NULL)
- INNER JOIN will return all users that commented
- LEFT OUTER JOIN (comment table on left) returns all comments associated with users, so it's same as INNER JOIN
- LEFT OUTER JOIN (user table on left) returns all users and associated comments

## OUTER JOIN across 3 tables

- e.g. user table, comment table, video table
- first, LOJ user table and comment table. This returns all users and all comments
- now do ROJ the LOJ'd table and video table. This returns all videos, all comments, but only some users

## Alias

- an alias is when you rename something. You can do this when writing SQL statements
- SELECT email AS contact, fn AS first name, ln AS last name FROM user

## SELF JOIN

- table referencing itself. useful for situations such as:
- e.g. user has a reference for a deal on a website
- user_id, fn, ln, email, referred_by columns. referred_by column references user_id. If you want the referred_by column to spit out an email or username instead of an id, then use self-join
- e.g. two tables, v1, and v2
- SELECT user AS v1 JOIN user AS v2
- e.g. v1 table (user_id, fn, ln, email) and v2 table (referred_by, person's email)
- SELECT v1.email AS referred, v2.email AS referrer FROM user AS v1 JOIN (defaults to inner join) user AS v2 ON v1.referred_by = v2.user_id
-
