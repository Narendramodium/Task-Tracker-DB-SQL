# Task-Tracker-DB-SQL
SQL scripts for database schema, data management, optimization, and backup for the Task Tracker App.

Script: 01_create_schema.sql.......
-- Create database
CREATE DATABASE TaskTracker;

-- Use the new database
USE TaskTracker;

-- User table
CREATE TABLE Users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Task table
CREATE TABLE Tasks (
    task_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    status ENUM('To Do', 'In Progress', 'Completed') DEFAULT 'To Do',
    due_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

-- Project table
CREATE TABLE Projects (
    project_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    start_date DATE,
    end_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Project members table
CREATE TABLE ProjectMembers (
    project_id INT,
    user_id INT,
    role ENUM('Owner', 'Collaborator'),
    PRIMARY KEY (project_id, user_id),
    FOREIGN KEY (project_id) REFERENCES Projects(project_id),
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);


Data Insertion Script.......
USE TaskTracker;

-- Insert users
INSERT INTO Users (username, email, password_hash)
VALUES
    ('john_doe', 'john@example.com', 'hashed_password1'),
    ('jane_smith', 'jane@example.com', 'hashed_password2');

-- Insert projects
INSERT INTO Projects (name, description, start_date, end_date)
VALUES
    ('Website Redesign', 'Redesigning the company website', '2024-01-01', '2024-06-30');

-- Insert tasks
INSERT INTO Tasks (title, description, status, due_date, user_id)
VALUES
    ('Design Mockups', 'Create initial mockups for the website', 'In Progress', '2024-02-15', 1),
    ('Write Content', 'Draft content for the new website', 'To Do', '2024-03-01', 2);

Data Validation and Constraints........
USE TaskTracker;

-- Add NOT NULL constraint to Project name
ALTER TABLE Projects MODIFY name VARCHAR(100) NOT NULL;

-- Add UNIQUE constraint to Users email
ALTER TABLE Users ADD CONSTRAINT unique_email UNIQUE (email);


