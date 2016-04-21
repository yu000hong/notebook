# Return Random Records in MySQL

The ability to return random records from a MySQL table is invaluable. Returning random records is helpful when:

- featuring items without showing favoritism to one
- testing different result sets in your PHP
- looking to display specific items in a non-specific order

The great part about selecting random records from your MySQL tables is that it's really easy.

### The Code

```sql
SELECT product_id, title, description
FROM products
WHERE active = 1
AND stock > 0
ORDER BY RAND()
LIMIT 4
```

The **`ORDER BY RAND()`** clause returns random records!

# 链接

[Return Random Records in MySQL](https://davidwalsh.name/mysql-random)
