

1. 检索数据库 table_name 的数据表 table_name 中 column_name列值中以一个数(包含以小数点开始的数)开始的所有值
   ```sql
   SELECT column_name
   FROM table_name
   WHERE column REGEXP '^[0-9\\.]'
   ORDER BY column_name;
   ```