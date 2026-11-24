## Understanding the Command Line, Terminal, and Shell

The command line is a basic text input interface which allows a user to enter "commands", usually in the form of a series of characters, and submit or execute them, usually by pressing the Enter key. You will most commonly see command line interfaces within a terminal.

A terminal is a special application that offers a command line interface to perform system-level commands beyond the basic read/write operations. You may also hear about "terminal emulators", which are essentially applications that wrap a basic terminal interface to offer additional features or functionality. But for most general purposes, you are probably safe to refer to these as "terminals" as well.

Finally, we will also need to talk about "shells". A shell is the software that wraps the command line and interprets your inputs as commands, returning the output.

## What Is a Relational Database

A relational database is **a collection of data organized into tables**. These tables are made of rows and columns.

**Rows represent individual records**. For example, if you're storing user data, each row could represent a specific user.

**Columns represent the attributes or fields that describe each record**. The table might include columns for common user attributes, such as name and national ID number.

One of the key characteristics of relational databases is that **they allow us to connect different pieces of information by linking tables through common attributes**, making it easier to see how they relate to each other.

Relational databases require a schema. **The schema defines the overall structure of the database**, including its tables, columns, data types, relationships, and constraints.

All tables in a relational database have a primary key, which is a unique identifier for each row. They enable relationships between tables via foreign keys.

A **query** is a request for specific data from the database.

## What Is SQL?

SQL stands for **Structured Query Language**. It was developed in the 1970s, when it was **initially known as the structured English query language (SEQUEL)**. This term was later shortened to SQL.

**SQL is a programming language** used for storing and managing data in relational databases.

## The Data Type Categories in SQL

There are six popular categories of data types in SQL:

- Numeric, such as `INTEGER`, `FLOAT`, `SERIAL`, and `DECIMAL`.

- Date and time, such as `TIMESTAMP`, `DATE`, and `TIME`.

- Character and string. These include `CHAR`, `VARCHAR`, and `TEXT`.

- Unicode, including `NTEXT`, and `NVARCHAR`. These are used to make sure that text will be stored and retrieved correctly, regardless of the characters' origin.

- Binary, used to store non-textual data, like images, audio, and video files.

- And other miscellaneous data types, such as `XML` and `TABLE`.

## The Main Types of Relationships

The "relational" aspect of "relational databases" comes from the fact that records in different tables can have different types of relationships.

There are five main types of relations:

- one-to-one
- one-to-many
- many-to-one
- many-to-many
- And self-referencing relationships (also known as recursive relationships).

### One-to-One Relationships

Two tables have a one-to-one relationship when each record in the first table can be associated with at most one record in the second table, and vice versa.

For example, let's say a company only assigns one vehicle per employee.

In that case, the database could have an employees table and a vehicles table, where each employee record corresponds to one, and only one, vehicle record in the database.

### One-to-Many and Many-to-One Relationships

In contrast, one-to-many relationships occur when one record in a table can be related to one or more records in another table.

For example, if we have a customers table and an orders table, each customer could be related to one or more orders but each order can only be related to one customer.

This is the same as the many-to-one relation, but from the opposite perspective. One or more orders can be related to one customer.

### Many-to-Many Relationships

Many-to-many relationships occur when a record in a table can be related to multiple records in another table and vice versa.

For example, let's analyze a database for a library. For simplicity purposes, let's assume that this database only has two tables: books and authors.

An author can write multiple books, and a single book can be written by multiple authors.

Implementing this type of relationship is a little bit more complex because the relational model at its core doesn't support many-to-many relationships directly.

To handle this, you would usually create an intermediate table, also known as a **junction table**.

This table solves these initial limitations by transforming the many-to-many relationship into two one-to-many relationships.

It creates a one-to-many relationship between the first table and the junction table and a one-to-many relationship between the second table and the junction table.

The name of the junction table is usually a combination of the entities. In the context of our example of books and authors, you may call it books_authors.

In the junction table itself, you would associate the authors and their books by creating two columns: a column for the author's id (author_id) and another column for the book's id (book_id).

These would be foreign keys referencing their corresponding primary keys in the authors and books tables, respectively.

With this junction table, you'll be able to query all the authors of a given book or all books written by a given author.

Using a junction table makes it easier to query the data, prevents redundancy, and simplifies the database schema. They are very helpful for implementing many-to-many relationships.

### Self-Referencing Relationships

And finally, we find self-referencing relationships, which occur when the records of a table can be related to other records on the same table. This is also known as a recursive relationship.

Understanding these relationships is essential for designing and modeling complex and efficient relational databases.
