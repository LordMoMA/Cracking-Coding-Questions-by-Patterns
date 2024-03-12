In PostgreSQL, regular locks are used to control concurrent access to data. Here are some common use cases:

### Row-level Locks: 

These are the most granular type of lock. They're used when a transaction needs to lock a specific row of a table. For example, when updating or deleting a row, PostgreSQL automatically acquires a row-level lock to prevent other transactions from modifying the same row at the same time.

```sql
BEGIN;
SELECT * FROM accounts WHERE id = 1 FOR UPDATE;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
COMMIT;
```

### Table-level Locks: 

These are used when a transaction needs to lock an entire table. For example, when altering the structure of a table (like adding a column), PostgreSQL acquires a table-level lock to prevent other transactions from accessing the table until the operation is complete.

```sql
BEGIN;
LOCK TABLE accounts IN SHARE MODE;
SELECT * FROM accounts WHERE balance < 0;
COMMIT;
```

### Share Locks: 

These are used when a transaction needs to read a piece of data and wants to ensure that the data doesn't change while it's being read. A share lock allows other transactions to also read the data, but not to modify it.

```sql
BEGIN;
SELECT * FROM accounts WHERE id = 1 FOR SHARE;
COMMIT;
```

### Exclusive Locks: 

These are used when a transaction needs to modify a piece of data and wants to ensure that no other transactions can read or modify the data until it's done. An exclusive lock prevents other transactions from acquiring either a share lock or an exclusive lock on the same data.

```sql
BEGIN;
SELECT * FROM accounts WHERE id = 1 FOR UPDATE;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
COMMIT;
```

### AccessExclusive Locks: 

These are the most restrictive type of lock. They're used for operationsthat need to prevent any other access to the table. For example, when dropping a table or an index, PostgreSQL acquires an AccessExclusive lock.

```sql
DROP TABLE accounts;
```

### Advisory Locks: 

These are user-defined locks that your application can acquire and release as needed. They're useful for cases where you need to synchronize access to a resource that's not directly associated with a database row or table.

```sql
BEGIN;
SELECT pg_advisory_lock(1);
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
SELECT pg_advisory_unlock(1);
COMMIT;
```

Remember that locks can lead to deadlocks if not used carefully. PostgreSQL will detect deadlocks and abort one of the transactions, but it's best to avoid them by always locking rows in a consistent order.