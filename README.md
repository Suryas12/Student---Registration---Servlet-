# Student---Registration---Servlet-
Student Registration Form using Java Servlet ,Jspand MYSQL 
-- Database create
CREATE DATABASE student_db;
USE student_db;

-- Table create
CREATE TABLE student_registration (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    roll_number VARCHAR(20) NOT NULL UNIQUE,
    department VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    phone VARCHAR(15) NOT NULL,
    gender VARCHAR(10),
    date_of_birth DATE,
    address TEXT,
    registered_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Check table
SELECT * FROM student_registration;

