# Basic-SQL-Queries

## 1. Creating database
- **Create a new database name TrainingExampleDB**
  -  ```sql
     CREATE DATABASE TrainingExampleDB;
     USE TrainingExampleDB;
- **Create tables for the db, including: customers, products, orders, order_items**
  - `customers` table:
    - 
    - ```sql
      CREATE TABLE customers (
	    id INT AUTO_INCREMENT PRIMARY KEY,
      email VARCHAR(255) NOT NULL,
      name VARCHAR(100) NOT NULL,
      created_at DATETIME DEFAULT CURRENT_TIMESTAMP
      )
  - `products` table:
    - 
    - ```sql
      CREATE TABLE products (
	    id INT AUTO_INCREMENT PRIMARY KEY,
      name VARCHAR(255) NOT NULL,
      price DECIMAL(10, 2) NOT NULL,
      stock INT DEFAULT 0
      )
  - `orders` table:
    - 
    - ```sql
      CREATE TABLE orders (
      id INT PRIMARY KEY AUTO_INCREMENT,
      customer_id INT NOT NULL,
      order_date DATETIME DEFAULT CURRENT_TIMESTAMP,
      status ENUM('pending', 'completed', 'cancelled') NOT NULL,
      FOREIGN KEY (customer_id) REFERENCES customers(id)
      );   
  - `order_items` table:
    - 
    - ```sql
      CREATE TABLE order_items (
      id INT PRIMARY KEY AUTO_INCREMENT,
      order_id INT NOT NULL,
      product_id INT NOT NULL,
      quantity INT NOT NULL,
      unit_price DECIMAL(10,2) NOT NULL,
      FOREIGN KEY (order_id) REFERENCES orders(id),
      FOREIGN KEY (product_id) REFERENCES products(id)
      );
- **Generate dummy data for database**
  - `customers` table:
    
      <img width="450" height="135" alt="image" src="https://github.com/user-attachments/assets/a533bab3-6610-41ec-b5d3-caf2c3c75a15" />
  - `products` table:
    
      <img width="445" height="133" alt="image" src="https://github.com/user-attachments/assets/05d0e88b-fcd1-4e2c-b5d6-40d7e4c24030" />

  - `orders` table:
    
      <img width="524" height="93" alt="image" src="https://github.com/user-attachments/assets/2060828d-5e8e-474e-baeb-8c542b6b4ff7" />

  - `order_items` table:
    
     <img width="481" height="179" alt="image" src="https://github.com/user-attachments/assets/e94d5be9-56d3-42e0-b97e-b6cd4b5d2e56" />

## 2. Basic SQL Queries
- **Get all the products price > 100000 and in stock > 0**
  
    - ```sql
      SELECT * FROM products WHERE price > 100000.00 AND stock > 0;
    - Result:
      
      <img width="443" height="90" alt="image" src="https://github.com/user-attachments/assets/e94d4381-a201-47c4-8266-d1009b0ffe24" />
- **Get detailed order list including: customer name, product name, quantity, unit price**
  
    - ```sql
      SELECT 
        c.name AS customer_name,
        p.name AS product_name,
        oi.quantity,
        oi.unit_price
      FROM order_items oi
      JOIN orders o ON oi.order_id = o.id
      JOIN customers c ON o.customer_id = c.id
      JOIN products p ON oi.product_id = p.id
      WHERE NOT o.status = 'cancelled'
    - Result:
      
      <img width="502" height="173" alt="image" src="https://github.com/user-attachments/assets/701987d3-4de2-4d9a-ab14-617c3f394ba0" />
- **Get the names of products that have been sold**
  
    - ```sql
      SELECT DISTINCT name AS product_name
      FROM products p
      JOIN order_items oi ON oi.product_id = p.id
      JOIN orders o ON o.id = oi.order_id
      WHERE o.status = 'completed'
    - Result:
      
      <img width="136" height="137" alt="image" src="https://github.com/user-attachments/assets/32a3782e-65e7-4ebe-b393-6f7129dc9968" />
- **Calculate the total number of orders for each customer**
  
    - ```sql
      SELECT 
        c.name AS customer_name,
        COUNT(o.id) AS total_orders
      FROM customers c
      LEFT JOIN orders o ON c.id = o.customer_id
      GROUP BY c.id
      ORDER BY total_orders
- **Calculate the total amount for each order**
  
    - ```sql
      SELECT 
        o.id AS order_id,
        SUM(oi.quantity * oi.unit_price) AS total_amount
      FROM orders o
      JOIN order_items oi ON o.id = oi.order_id
      WHERE NOT o.status = 'cancelled'
      GROUP BY o.id
      ORDER BY o.id
    - Result:
      
      <img width="184" height="74" alt="image" src="https://github.com/user-attachments/assets/f464f5c7-0d79-417d-9730-50bfac2491b1" />
- **Get customers who have never ordered**
  
    - ```sql
      SELECT 
        c.id AS customer_id,
        c.name AS customer_name
      FROM customers c
      LEFT JOIN orders o ON c.id = o.customer_id
      WHERE o.customer_id IS NULL;
    - Result:
 
      <img width="210" height="84" alt="image" src="https://github.com/user-attachments/assets/20e68080-8dd2-4e04-90bb-258b755142de" />
- **Get the most sold product**
  
    - ```sql
      SELECT 
        p.name AS product_name,
        SUM(oi.quantity) AS total_quantity_sold
      FROM products p
      JOIN order_items oi ON p.id = oi.product_id
      JOIN orders o ON o.id = oi.order_id
      WHERE o.status = 'completed'
      GROUP BY p.id, p.name
      ORDER BY total_quantity_sold DESC
      LIMIT 1;
    - Result:
 
      <img width="279" height="53" alt="image" src="https://github.com/user-attachments/assets/1e639007-bdf7-44de-9862-0a2076cbc0d9" />





      

  
  
  
  

