# DB Designing Exercises

## 1. Exercise 1: Library Management
Design a simple library management system. The system needs to manage information about books, users, and book borrowing transactions.
  ```sql
      USE trainingexampledb;
      CREATE TABLE books (
  	    id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(255) NOT NULL,
        stock INT NOT NULL
      );
      CREATE TABLE users (
      	id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(100) NOT NULL
      );
      CREATE TABLE borrowings (
      	id INT PRIMARY KEY AUTO_INCREMENT, 
        book_id INT NOT NULL,
        user_id INT NOT NULL,
	borrow_date DATETIME DEFAULT CURRENT_TIMESTAMP,
	return_date DATETIME,
	amount INT NOT NULL,
        FOREIGN KEY (book_id) REFERENCES books(id),
      	FOREIGN KEY (user_id) REFERENCES users(id)
      )
  ```


## 2. Exercise 2: Online Sales Management
Design a sales management system.
Need to manage information about customers, products, orders, order details and payments.
  ```sql
    USE trainingexampledb;
    
    CREATE TABLE users (
    	id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(155) NOT NULL
    );
    
    CREATE TABLE items (
    	id INT PRIMARY KEY AUTO_INCREMENT,
       name VARCHAR(255) NOT NULL,
       price DECIMAL(10,2) NOT NULL,
       stock INT NOT NULL
    ); 
    
    CREATE TABLE orders (
    	id INT PRIMARY KEY AUTO_INCREMENT,
        user_id INT NOT NULL,
        total_amount DECIMAL(10,2) NOT NULL,
        date DATETIME DEFAULT CURRENT_TIMESTAMP,
        status ENUM('completed', 'pending', 'cancelled') NOT NULL DEFAULT 'pending',
        FOREIGN KEY (user_id) REFERENCES users(id)
    );
    
    CREATE TABLE order_items (
    	id INT PRIMARY KEY AUTO_INCREMENT,
        order_id INT NOT NULL,
        item_id INT NOT NULL,
        quantity INT NOT NULL,
        unit_price DECIMAL(10,2),
        FOREIGN KEY (order_id) REFERENCES orders(id),
        FOREIGN KEY (item_id) REFERENCES items(id)
    );
    
    CREATE TABLE payments (
    	id INT PRIMARY KEY AUTO_INCREMENT,
        order_id INT NOT NULL,
        total_amount DECIMAL(10,2) NOT NULL,
        status ENUM('paid', 'unpaid') NOT NULL,
        payment_method ENUM('cash', 'card', 'bank transfer') NOT NULL DEFAULT 'cash',
        FOREIGN KEY (order_id) REFERENCES orders(id)
    );
    
    CREATE TABLE order_details (
    	id INT PRIMARY KEY AUTO_INCREMENT,
        order_id INT NOT NULL,
        payment_id INT NOT NULL,
        shipping_date DATETIME NOT NULL,
        FOREIGN KEY (order_id) REFERENCES orders(id),
        FOREIGN KEY (payment_id) REFERENCES payments(id)
    );
```

## 3. Online Course Management
Design an online course management system for "n" schools. Company A wants to provide online course management services, and schools will register for this service. The system needs to manage users (students, instructors), courses, lessons, student enrollment and learning processes, along with reviews and comments for each school.
  ```sql
    USE trainingexampledb;

CREATE TABLE users (
	id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(155) NOT NULL,
    status ENUM('student', 'teacher') DEFAULT 'student',
    school VARCHAR(155) NOT NULL
);

CREATE TABLE courses (
	id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(155) NOT NULL,
    teacher_id INT NOT NULL,
    start_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    end_date DATETIME NOT NULL,
    FOREIGN KEY (teacher_id) REFERENCES users(id)
);

CREATE TABLE lessons (
	id INT PRIMARY KEY AUTO_INCREMENT,
    course_id INT NOT NULL,
    title VARCHAR(255),
    duration TIMESTAMP NOT NULL
);

CREATE TABLE enrollments (
	id INT PRIMARY KEY AUTO_INCREMENT,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    date DATETIME DEFAULT CURRENT_TIMESTAMP, 
    progress TINYINT NOT NULL DEFAULT 0 CHECK (progress >= 0 AND progress <= 100),
    status ENUM('enrolled', 'cancelled'), 
    FOREIGN KEY (student_id) REFERENCES users(id),
    FOREIGN KEY (course_id) REFERENCES courses(id)
);

CREATE TABLE progresses (
	id INT PRIMARY KEY AUTO_INCREMENT,
    student_id INT NOT NULL,
    lesson_id INT NOT NULL,
    status ENUM('done', 'in progress') DEFAULT 'in progress',
    date DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (student_id) REFERENCES users(id),
    FOREIGN KEY (lesson_id) REFERENCES lessons(id)
);

CREATE TABLE feedbacks (
	id INT PRIMARY KEY AUTO_INCREMENT,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    content VARCHAR(500) NOT NULL,
    date DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (student_id) REFERENCES users(id),
    FOREIGN KEY (course_id) REFERENCES courses(id)
);



    
