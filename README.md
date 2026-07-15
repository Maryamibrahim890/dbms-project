# Library Management System — DBMS Project

A relational database project that manages books, authors, publishers, members, staff, and book issue/return transactions for a library.

## Overview

This project models a library's day-to-day operations: tracking which books exist, who wrote/published them, who the members are, which staff process transactions, and the full lifecycle of a book being issued and returned — including automatic copy-count updates and late-return fines.

## Tech Stack

- **Database**: MySQL (5.7+ / 8.0)
- **Tools**: MySQL Workbench / phpMyAdmin / any MySQL client

## Entity-Relationship Summary

| Entity | Description |
|---|---|
| Author | Writers of the books |
| Publisher | Publishing houses |
| Book | Book catalog, linked to Author and Publisher |
| Member | Library members who borrow books |
| Staff | Employees who process book issues |
| Issue | Transaction record linking Book, Member, and Staff |

**Relationships**
- Author → Book: one-to-many
- Publisher → Book: one-to-many
- Book, Member, Staff → Issue: one-to-many each (Issue acts as the junction/transaction table)

## Database Design

- Normalized up to **3NF** — author and publisher details are kept in separate tables to avoid transitive dependency and data redundancy in the Book table.
- Referential integrity enforced via `FOREIGN KEY` constraints with `ON DELETE`/`ON UPDATE` rules.
- Data validity enforced via `NOT NULL`, `UNIQUE`, `CHECK`, and `DEFAULT` constraints.

## Features Implemented

- **Triggers**
  - `trg_after_issue` — automatically decrements a book's available copies when it's issued.
  - `trg_after_return` — automatically restores available copies and calculates a late-return fine (Rs. 5/day) when a book is returned.
- **Stored Procedure**
  - `IssueBook` — issues a book only if a copy is available, preventing over-issuing.
- **View**
  - `Overdue_Books` — lists all currently unreturned books that are past their due date.
- **Queries**
  - Multi-table joins, aggregate functions (`COUNT`, `SUM`), `GROUP BY` / `HAVING`, subqueries, and `ORDER BY`/`LIMIT` ranking queries.

## Setup Instructions

1. Open the `library_management_dbms.sql` file in your MySQL client.
2. Run the entire script — it will:
   - Create the `library_db` database
   - Create all 6 tables with constraints
   - Insert sample data
   - Create the triggers, stored procedure, and view
3. Run any of the example queries at the bottom of the script to test the system.

```sql
-- Example: issue a book safely through the stored procedure
CALL IssueBook(1, 2, 1, '2026-07-01', '2026-07-15');

-- Example: check overdue books
SELECT * FROM Overdue_Books;
```

## File Structure

```
├── library_management_dbms.sql   # Full schema, sample data, triggers, procedure, view, queries
├── DBMS_Viva_Guide.md            # Concept explanations and likely viva Q&A
└── README.md                     # This file
```

## Possible Future Enhancements

- Add a `Fine_Payment` table to track when/how fines are paid
- Add indexes on frequently searched columns (e.g. `Book.title`, `Member.email`)
- Add a role-based login system for staff vs admin
- Build a front-end (e.g. using PHP, Java, or Python) to interact with this database
