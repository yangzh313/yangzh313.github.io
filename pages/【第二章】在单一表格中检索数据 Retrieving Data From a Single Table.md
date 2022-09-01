- The SELECT Statement
- The SELECT Clause
	- AS设置列别名
	- 无视大小写
- The WHERE Clause
- The AND, OR and NOT Operator
	- AND优先级高于OR
- The IN Operator
- The BETWEEN Operator
- The LIKE Operator
- THE REGEXP OPERATOR
	- REGEXP
	- ^，开头；$，结尾；|，多个搜索模式；[]，任意一个；[a-c]：abc，范围
- THE IS NULL OPERATOR
	- IS NULL
	- IS NOT NULL
- The ORDER BY clause
	- ORDER BY first_name DESC
	- ORDER BY state, first_name
	- ```
	  SELECT first_name, last_name, 10 AS points
	  FROM customers c 
	  ORDER BY points, first_name (ORDER BY 3, 1) --位置
	  
	  SELECT order_id, product_id, quantity, unit_price, quantity * unit_price AS total_price
	  FROM order_items oi 
	  WHERE order_id = 2
	  ORDER BY quantity * unit_price DESC 
	  ```
- The LIMIT clause
	- LIMIT 300
	- LIMIT 6, 3 跳过6条记录获取3条记录
	- ```
	  SELECT *
	  FROM customers c 
	  ORDER BY points DESC 
	  LIMIT 3
	  ```
-