

# 🔥 4. SQL / DB Validation (Likely)

### Questions:

* How do you validate API data in DB?
* SQL join query
* Duplicate records query
* How do you test transactional rollback?
* SQL vs NoSQL?

### Example:

```sql id="g8kg3g"
Find duplicate emails in users table
```

```sql
SELECT email, COUNT(*)
FROM users
GROUP BY email
HAVING COUNT(*) > 1;
```
