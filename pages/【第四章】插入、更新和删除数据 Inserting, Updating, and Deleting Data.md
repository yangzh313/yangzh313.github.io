- Column Attributes
- Inserting a Row
	- ```
	  -- Get the orders placed this year
	  USE sql_store
	  
	  INSERT INTO customers (
	  	last_name,
	  	first_name,
	  	birth_date,
	  	address,
	  	city,
	  	state)
	  VALUES (
	  	'John', 
	  	'Smith', 
	  	'1992-03-13', 
	  	'address', 
	  	'city', 
	  	'CA')
	  ```
- Inserting Multiple Rows
	- ```
	  INSERT INTO shippers (name)
	  VALUES ('SHI1'),
	  		('SHI2')
	  ```
- Inserting Hierarchical Rows
	- MySQL内置功能就是一堆可以反复使用的代码， SELECT LAST_INSERT_ID()
	- ```
	  INSERT INTO orders (customer_id, order_date, status)
	  VALUES (1, '2019-01-02', 1);
	  
	  INSERT INTO order_items
	  VALUES
	  (LAST_INSERT_ID(), 1, 1, 2.95),
	  (LAST_INSERT_ID(), 2, 1, 3.95)
	  ```
- Creating a copy of a table
	- INSERT INTO orders_archived
	  SELECT *
	  FROM orders 
	  WHERE order_data < '2019-01-01'