# **Product User Database - Active Users Tracking** 🗂️👤

## **Overview** 📝
This project focuses on building a simple database for tracking active users of a product using **SQL**. It consists of two tables: `users` and `logins`. The goal is to track user logins and identify the active users based on their recent login activity.

## **Features** ⚙️
- **User Management**: Stores user details such as usernames and emails 👥.
- **Login History**: Tracks each user’s login time to identify active users ⏰.
- **Active User Query**: Finds users who have logged in within the last 7 days 🔄.

## **Database Schema** 🏛️
### **1. Users Table** 🧑‍💻
The `users` table stores user details such as their `user_id`, `username`, `email`, and the time they were created.

```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### **2. Logins Table** 🔐
The `logins` table stores each login event with the associated `user_id`, `login_time`, and a foreign key reference to the `users` table.

```sql
CREATE TABLE logins (
    login_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    login_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE
);
```

## **Sample Data** 🗃️
### **Insert Users** 👥
```sql
INSERT INTO users (username, email) VALUES
('Alice', 'alice@example.com'),
('Bob', 'bob@example.com'),
('Charlie', 'charlie@example.com'),
('David', 'david@example.com'),
('Eve', 'eve@example.com');
```

### **Insert Logins** 🔑
```sql
INSERT INTO logins (user_id, login_time) VALUES
(1, '2024-03-20 10:15:00'),
(2, '2024-03-21 12:30:00'),
(3, '2024-03-22 14:45:00'),
(1, '2024-03-23 16:00:00'),
(2, '2024-03-24 18:15:00'),
(4, '2024-03-25 20:30:00'),
(5, '2024-03-26 22:45:00'),
(3, '2024-03-27 09:00:00'),
(1, '2024-03-28 11:15:00'),
(5, '2024-03-28 13:30:00');
```

## **Key Queries** 🔍
### **1. Query to Find Last Login of Each User** ⏳
This query will find the last login time for each user:

```sql
SELECT u.user_id, u.username, MAX(l.login_time) AS last_login
FROM users u
JOIN logins l ON u.user_id = l.user_id
GROUP BY u.user_id, u.username
ORDER BY last_login DESC;
```

### **2. Query to Find Active Users in the Last 7 Days** 📅
This query finds users who logged in within the last 7 days:

```sql
SELECT u.user_id, u.username, MAX(l.login_time) AS last_login
FROM users u
JOIN logins l ON u.user_id = l.user_id
WHERE l.login_time >= NOW() - INTERVAL 7 DAY
GROUP BY u.user_id, u.username
ORDER BY last_login DESC;
```

## **Technologies Used** 🛠️
- **SQL**: Used for database creation, data insertion, and querying.
- **MySQL / MariaDB / SQL Workbench**: You can use any SQL-compatible platform to execute these queries.


