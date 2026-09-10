## PART A — Core Concepts

### A1. What is a database?

A database is an organized collection of related information stored electronically so that it can be easily added, searched, updated, and managed. It helps applications keep data safely and consistently instead of losing information when a program stops running.

### A2. Program variable vs database data

A program variable temporarily stores data while a program is running, usually in RAM. Database data is stored for long-term use and can remain available after the program closes.

Practical differences:

1. Variables are usually temporary; database records are persistent.
2. Variables belong to a running program; databases can be accessed by different programs/users.
3. Databases are designed for large amounts of organized data.

### A3. Why does a backend need a database if the server has RAM?

RAM is temporary memory used while the server is running. Data stored only in RAM can disappear when the server shuts down or restarts. A database provides permanent storage for important application data. It also allows data to be searched, updated, related, and managed efficiently.

### A4. ACID Properties
Atomicity:A transaction either completes fully or does not happen at all.A bank transfer must debit one account and credit the other, not just one.

Consistency:A transaction must leave the database following its rules.A student cannot be registered for a course that does not exist.

Isolation:Simultaneous transactions should not incorrectly interfere with each other.Two people buying the last available ticket should not both receive it.                   |

Durability:Once a transaction is successfully saved, it should remain saved.After an online payment succeeds, the payment record remains even after a server restart.

### A5. Primary Key and Foreign Key

Primary Key (PK):A column that uniquely identifies each record in a table.

Example:`StudentID` uniquely identifies each student.

Foreign Key (FK): A column that connects one table to another by referencing its primary key.

Example: `StudentID` in an `Enrollments` table can reference `StudentID` in the `Students` table.

# PART B

### B1. Redundancy

Repeated values include:

Pride and Prejudice — 2 times
Jane Austen — 2 times
1775 — 2 times
1984 — 2 times
George Orwell — 3 times
1903 — 3 times
Ada L. — 3 times
ada@example.com— 3 times

### B2. Three problems

1. Update anomaly:
If George Orwell's birth year changes, it must be updated in three rows. Missing one creates inconsistent data.

2. Insertion anomaly:
We cannot easily add a new author without also creating a book/loan record.

3. Deletion anomaly:
If all records for a particular book are deleted, important information about its author may also be lost.

### B3. Normalization

1NF — Yes.
Each cell contains a single value and there are no repeating groups.

2NF — No.
The table mixes book, author, member, and loan information, causing attributes to depend on only parts of the intended loan relationship.

3NF — No.
There are transitive dependencies, such as:

`AuthorName → AuthorBirth`

and

`MemberEmail → MemberName`

So author and member information should be separated.

---

## B4. Properly Normalised Tables

### 1. Authors

| Column     | Data Type    | Key      |
| ---------- | ------------ | -------- |
| AuthorID   | INT          | **PK**   |
| AuthorName | VARCHAR(100) | NOT NULL |
| BirthYear  | INT          |          |

### 2. Books

| Column   | Data Type    | Key                       |
| -------- | ------------ | ------------------------- |
| BookID   | INT          | PK                    |
| Title    | VARCHAR(200) | NOT NULL                  |
| AuthorID | INT          | FK → Authors.AuthorID |

### 3. Members

| Column      | Data Type    | Key      |
| ----------- | ------------ | -------- |
| MemberID    | INT          | PK   |
| MemberName  | VARCHAR(100) | NOT NULL |
| MemberEmail | VARCHAR(150) | UNIQUE   |

### 4. Loans

| Column     | Data Type | Key                       |
| ---------- | --------- | ------------------------- |
| LoanID     | INT       | PK                        |
| BookID     | INT       | FK → Books.BookID         |
| MemberID   | INT       | FK → Members.MemberID     |
| LoanDate   | DATE      | NOT NULL                  |
| ReturnDate | DATE      | NULL                      |

### B5. Simple ER Diagram

```text
AUTHORS
AuthorID (PK)
AuthorName
BirthYear
     |
     | 1:N
     |
BOOKS
BookID (PK)
Title
AuthorID (FK)
     |
     | 1:N
     |
LOANS
LoanID (PK)
BookID (FK)
MemberID (FK)
LoanDate
ReturnDate
     |
     | N:1
     |
MEMBERS
MemberID (PK)
MemberName
MemberEmail
```

---

# PART C — Mini Project

## Option 1: Simple Library

### C1. Entities/Tables

1. `Authors`
2. `Books`
3. `Members`
4. `Loans`

### C2 & C3. Columns, Data Types and Keys

Authors

* `AuthorID` — INT, PK
* `AuthorName` — VARCHAR(100), NOT NULL
* `BirthYear` — INT

Books

* `BookID` — INT, PK
* `Title` — VARCHAR(200), NOT NULL
* `AuthorID` — INT, FK → `Authors.AuthorID`

Members

* `MemberID` — INT, PK
* `MemberName` — VARCHAR(100), NOT NULL
* `MemberEmail` — VARCHAR(150), UNIQUE, NOT NULL

Loans

* `LoanID` — INT, PK
* `BookID` — INT, FK → `Books.BookID`
* `MemberID` — INT, FK → `Members.MemberID`
* `LoanDate` — DATE, NOT NULL
* `ReturnDate` — DATE, NULL

### C4. Relationships

* Author 1:N Books — one author can write many books.
* Book 1:N Loans — one book can be loaned many times over time.
* Member 1:N Loans — one member can have many loans.
* Members N:M Books — resolved through the `Loans` table.

### C5. Why the design satisfies 1NF, 2NF and 3NF

* 1NF: Every column contains atomic values.
* 2NF: Non-key columns depend on the whole primary key.
* 3NF: Non-key columns depend only on their table's primary key, not on another non-key column.

### C6. Five Sample Loan Rows

| LoanID | BookID | MemberID | LoanDate   | ReturnDate |
| -----: | -----: | -------: | ---------- | ---------- |
|      1 |      1 |        1 | 2026-09-01 | 2026-09-10 |
|      2 |      2 |        2 | 2026-09-02 | NULL       |
|      3 |      3 |        1 | 2026-09-03 | 2026-09-08 |
|      4 |      4 |        3 | 2026-09-04 | NULL       |
|      5 |      1 |        2 | 2026-09-05 | NULL       |

For your paper: draw four boxes — `Authors`, `Books`, `Members`, and `Loans` — then connect them using the 1:N relationships shown above.
