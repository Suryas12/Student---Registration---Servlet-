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

package com.student;

import java.io.*;
import java.sql.*;
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;

@WebServlet("/StudentServlet")
public class StudentServlet extends HttpServlet {

     DB Connection details
    private static final String DB_URL = "jdbc:mysql://localhost:3306/student_db";
    private static final String DB_USER = "root";
    private static final String DB_PASSWORD = "yourpassword"; // உங்க password போடுங்க

    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {

        response.setContentType("text/html");

         Form data எடுக்கிறோம்
        String name = request.getParameter("name");
        String rollNumber = request.getParameter("roll_number");
        String department = request.getParameter("department");
        String email = request.getParameter("email");
        String phone = request.getParameter("phone");
        String gender = request.getParameter("gender");
        String dob = request.getParameter("dob");
        String address = request.getParameter("address");

        Connection con = null;
        PreparedStatement ps = null;

        try {
            JDBC Driver load
            Class.forName("com.mysql.cj.jdbc.Driver");

             DB Connect
            con = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD);

            SQL Query
            String sql = "INSERT INTO student_registration (name, roll_number, department, email, phone, gender, date_of_birth, address) VALUES (?, ?, ?, ?, ?, ?, ?, ?)";

            ps = con.prepareStatement(sql);
            ps.setString(1, name);
            ps.setString(2, rollNumber);
            ps.setString(3, department);
            ps.setString(4, email);
            ps.setString(5, phone);
            ps.setString(6, gender);
            ps.setString(7, dob);
            ps.setString(8, address);

            int result = ps.executeUpdate();

            if (result > 0) {
                // Success page-ku redirect
                request.setAttribute("studentName", name);
                request.setAttribute("rollNumber", rollNumber);
                request.setAttribute("department", department);
                RequestDispatcher rd = request.getRequestDispatcher("success.jsp");
                rd.forward(request, response);
            } else {
                response.getWriter().println("<h3>Registration Failed! Try again.</h3>");
            }

        } catch (ClassNotFoundException e) {
            e.printStackTrace();
            response.getWriter().println("<h3>Driver not found: " + e.getMessage() + "</h3>");
        } catch (SQLException e) {
            e.printStackTrace();
            response.getWriter().println("<h3>DB Error: " + e.getMessage() + "</h3>");
        } finally {
            // Connection close
            try {
                if (ps != null) ps.close();
                if (con != null) con.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.sendRedirect("index.jsp");
    }
}
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Registration</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            background: #f0f4f8;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            background: #ffffff;
            padding: 35px 40px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.15);
            width: 500px;
        }

        h2 {
            text-align: center;
            color: #0A2647;
            margin-bottom: 25px;
            font-size: 22px;
            border-bottom: 3px solid #2C74B3;
            padding-bottom: 10px;
        }

        .form-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #144272;
            font-size: 14px;
        }

        input[type="text"],
        input[type="email"],
        input[type="date"],
        select,
        textarea {
            width: 100%;
            padding: 10px 12px;
            border: 1px solid #C5D8EC;
            border-radius: 6px;
            font-size: 14px;
            color: #1A1A2E;
            background: #f9fbfd;
            transition: border 0.3s;
        }

        input:focus, select:focus, textarea:focus {
            border-color: #2C74B3;
            outline: none;
            background: #ffffff;
        }

        .gender-group {
            display: flex;
            gap: 20px;
            margin-top: 5px;
        }

        .gender-group label {
            font-weight: normal;
            display: flex;
            align-items: center;
            gap: 5px;
        }

        button[type="submit"] {
            width: 100%;
            padding: 12px;
            background: #0A2647;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 10px;
            transition: background 0.3s;
        }

        button[type="submit"]:hover {
            background: #2C74B3;
        }

        .reset-btn {
            width: 100%;
            padding: 10px;
            background: #ffffff;
            color: #0A2647;
            border: 2px solid #0A2647;
            border-radius: 6px;
            font-size: 14px;
            cursor: pointer;
            margin-top: 8px;
            transition: all 0.3s;
        }

        .reset-btn:hover {
            background: #f0f4f8;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>🎓 Student Registration Form</h2>

        <form action="StudentServlet" method="post" onsubmit="return validateForm()">

            <div class="form-group">
                <label for="name">Full Name *</label>
                <input type="text" id="name" name="name" placeholder="Enter your full name" required />
            </div>

            <div class="form-group">
                <label for="roll_number">Roll Number *</label>
                <input type="text" id="roll_number" name="roll_number" placeholder="e.g. 21CSE001" required />
            </div>

            <div class="form-group">
                <label for="department">Department *</label>
                <select id="department" name="department" required>
                    <option value="">-- Select Department --</option>
                    <option value="CSE">Computer Science and Engineering</option>
                    <option value="ECE">Electronics and Communication</option>
                    <option value="EEE">Electrical and Electronics</option>
                    <option value="MECH">Mechanical Engineering</option>
                    <option value="CIVIL">Civil Engineering</option>
                    <option value="IT">Information Technology</option>
                </select>
            </div>

            <div class="form-group">
                <label for="email">Email Address *</label>
                <input type="email" id="email" name="email" placeholder="example@email.com" required />
            </div>

            <div class="form-group">
                <label for="phone">Phone Number *</label>
                <input type="text" id="phone" name="phone" placeholder="+91 XXXXXXXXXX" maxlength="15" required />
            </div>

            <div class="form-group">
                <label>Gender</label>
                <div class="gender-group">
                    <label><input type="radio" name="gender" value="Male" /> Male</label>
                    <label><input type="radio" name="gender" value="Female" /> Female</label>
                    <label><input type="radio" name="gender" value="Other" /> Other</label>
                </div>
            </div>

            <div class="form-group">
                <label for="dob">Date of Birth</label>
                <input type="date" id="dob" name="dob" />
            </div>

            <div class="form-group">
                <label for="address">Address</label>
                <textarea id="address" name="address" rows="3" placeholder="Enter your address"></textarea>
            </div>

            <button type="submit">✅ Register Student</button>
            <button type="reset" class="reset-btn">🔄 Reset Form</button>

        </form>
    </div>

    <script>
        function validateForm() {
            var phone = document.getElementById("phone").value;
            var phonePattern = /^[0-9]{10}$/;

            if (!phonePattern.test(phone.replace(/\s/g, '').replace('+91', ''))) {
                alert("Please enter a valid 10-digit phone number!");
                return false;
            }
            return true;
        }
    </script>
</body>
</html>
