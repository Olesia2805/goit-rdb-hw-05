# goit-rdb-hw-05

## Task 1. Write a SQL query that displays the order_details table and the customer_id field from the orders table for each record field in the order_details table, respectively. This should be done using a nested query in a SELECT statement.
----------------------------------------------------------
|	SELECT *, (SELECT customer_id                          |
|	           FROM orders                                 |
|	           WHERE orders.id = order_details.order_id)   |
|	AS customer_id                                         |
|	FROM order_details;                                    |
----------------------------------------------------------

## Task 2. Write a SQL query that will display the order_details table. Filter the results so that the corresponding record from the orders table meets the condition shipper_id=3. This should be done using a nested query in the WHERE clause.
-------------------------------------------------------------------------
|  SELECT order_details.id, order_details.order_id AS order_details_id, |
|         orders.id AS orders_id, orders.shipper_id FROM order_details  |
|  JOIN orders ON orders.id = order_details.order_id                    |
|  WHERE order_id IN (SELECT id FROM orders WHERE shipper_id=3);        |
-------------------------------------------------------------------------

## Task 3. Write a SQL query nested in a FROM clause that will select rows with the quantity>10 condition from the order_details table. For the data obtained, find the average value of the quantity field - group by order_id.

---------------------------------------------------------------
|	SELECT temp_table.order_id,                                 |
|	       ROUND(AVG(temp_table.quantity), 2) AS avg_quantity   |
|	FROM (SELECT order_id, quantity                             |
|	      FROM order_details                                    |
|	      WHERE quantity > 10) AS temp_table                    |
|	GROUP BY order_id;                                          |
---------------------------------------------------------------

## Task 4. Solve task 3 by using the WITH statement to create a temporary table temp. If your version of MySQL is earlier than 8.0, create this query similarly to the one in the outline.

--------------------------------------------------------------
|	WITH temp_table AS (SELECT order_id, quantity              |
|		                  FROM order_details                     |
|	                    WHERE quantity > 10)                   |
|	SELECT temp_table.order_id,                                |
|	       ROUND(AVG(temp_table.quantity), 2) AS avg_quantity  |
|	FROM temp_table                                            |
|	GROUP BY order_id;                                         |
--------------------------------------------------------------

## Task 5. Create a function with two parameters that will divide the first parameter by the second. Both parameters and the returned value must be of type FLOAT. Use the DROP FUNCTION IF EXISTS construct. Apply the function to the quantity attribute of the order_details table. The second parameter can be any number of your choice.

------------------------------------------------------------------------
|	DROP FUNCTION IF EXISTS DivideQuantity;                              |
|                                                                      |
|	DELIMITER //                                                         |
|	CREATE FUNCTION DivideQuantity (firstValue FLOAT, secondValue FLOAT) |
|	RETURNS FLOAT                                                        |
|	DETERMINISTIC                                                        |
|	BEGIN                                                                |
|	    DECLARE result FLOAT;                                            |
|	    IF secondValue = 0 THEN                                          |
|	        RETURN NULL;                                                 |
|	    ELSE                                                             |
|	        SET result = firstValue / secondValue;                       |
|	        RETURN result;                                               |
|	    END IF;                                                          |
|	END //                                                               |
|	DELIMITER ;                                                          |
|                                                                      |
|	SELECT quantity,                                                     |
|	       DivideQuantity(quantity, 2),                                  |
|	       DivideQuantity(quantity, 3),                                  |
|	       DivideQuantity(quantity, 13)                                  |
|	FROM order_details                                                   |
------------------------------------------------------------------------
