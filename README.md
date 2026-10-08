# Task Management System

A secure, responsive, full-stack **Task Management System** built with **Node.js, Express.js, RESTful APIs, MySQL, JWT Authentication, bcrypt password hashing, and modern CSS3 Glassmorphism UI**.

---

## 🌟 Features

- 🔒 **User Authentication**: Secure Registration & Login with `bcryptjs` password hashing and `JWT` session tokens.
- 📊 **Interactive Dashboard**: Real-time counter statistics for Total, Pending, In-Progress, and Completed tasks.
- 📝 **Full Task CRUD**: Create, View, Edit (with modern modal window), and Delete tasks with instant confirmation.
- 🎯 **Priority & Status Management**:
  - **Statuses**: `Pending`, `In Progress`, `Completed`
  - **Priorities**: `Low`, `Medium`, `High`
- 🔍 **Search & Filter & Sort**:
  - Live search by task title
  - Filter by status & priority
  - Sort by due date, created date, priority, or title
- 🛡️ **Protected REST APIs**: Express JWT authentication middleware preventing unauthorized task access.
- 🎨 **Modern Responsive UI**: Built with a sleek dark-mode glassmorphism theme, FontAwesome icons, and custom micro-animations.

---

## 🛠️ Technology Stack

- **Backend**: Node.js, Express.js, `mysql2`, `jsonwebtoken`, `bcryptjs`, `dotenv`, `cors`
- **Database**: MySQL Server
- **Frontend**: HTML5, CSS3 (Glassmorphism design system), JavaScript (Fetch API, ES6 Modules)
- **Development & Testing**: VS Code, Git, Postman

---

## 📂 Project Structure

```text
Task Management System/
│
├── backend/
│   ├── config/
│   │   └── db.js               # MySQL2 Connection Pool
│   ├── controllers/
│   │   ├── authController.js   # Register, Login & Profile Logic
│   │   └── taskController.js   # CRUD & Stats Logic
│   ├── middleware/
│   │   └── authMiddleware.js   # JWT Verification Middleware
│   ├── models/
│   │   ├── userModel.js        # User SQL Queries
│   │   └── taskModel.js        # Task SQL Queries
│   ├── routes/
│   │   ├── authRoutes.js       # /api/auth Endpoints
│   │   └── taskRoutes.js       # /api/tasks Endpoints
│   ├── app.js                  # Express App, Middleware & Static Files
│   ├── server.js               # Server Entry Point
│   └── .env                    # Environment Variables (Git ignored)
│
├── frontend/
│   ├── index.html              # Landing Portal
│   ├── login.html              # User Login Page
│   ├── register.html           # User Registration Page
│   ├── dashboard.html          # Task Overview & Summary
│   ├── tasks.html              # Task List with Filters & Edit Modal
│   ├── add-task.html           # Create Task Form
│   ├── css/
│   │   └── style.css           # Custom Glassmorphism UI Stylesheet
│   └── js/
│       ├── auth.js             # Authentication Fetch API
│       ├── dashboard.js        # Dashboard Stats Fetch API
│       └── tasks.js            # Task CRUD Fetch API & Handlers
│
├── database/
│   └── schema.sql              # Database Tables & Foreign Keys Schema
├── .gitignore
└── README.md
```

---

## 🚀 Step-by-Step Installation & Setup

### 1. Prerequisite Checklist
Ensure you have installed:
- [Node.js](https://nodejs.org/) (v16+ recommended)
- [MySQL Server](https://dev.mysql.com/downloads/mysql/) or XAMPP/WAMP

---

### 2. Database Setup

1. Open your MySQL client (MySQL Workbench, phpMyAdmin, or Command Line):
```bash
mysql -u root -p
```
2. Execute the provided SQL script located in `database/schema.sql`:
```sql
CREATE DATABASE IF NOT EXISTS task_management;
USE task_management;

CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(150) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

CREATE TABLE IF NOT EXISTS tasks (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    title VARCHAR(200) NOT NULL,
    description TEXT,
    priority ENUM('Low', 'Medium', 'High') DEFAULT 'Medium',
    status ENUM('Pending', 'In Progress', 'Completed') DEFAULT 'Pending',
    due_date DATE NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

---

### 3. Backend Setup & Configuration

1. Navigate to the `backend` directory:
```bash
cd backend
```
2. Install npm dependencies:
```bash
npm install
```
3. Configure environment variables in `backend/.env`:
```env
PORT=5000
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_mysql_password
DB_NAME=task_management
JWT_SECRET=super_secret_task_management_jwt_key_2026
```

---

### 4. Running the Application

Run the dev server:
```bash
npm run dev
# Or for production:
npm start
```
- Open your browser and navigate to: **`http://localhost:5000`**

---

## 📡 REST API Documentation

### Authentication Endpoints

| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/auth/register` | Register a new user | ❌ No |
| `POST` | `/api/auth/login` | Log in and receive JWT token | ❌ No |
| `GET` | `/api/auth/me` | Get current logged in user details | ✅ Yes (`Bearer <token>`) |

#### Sample Register Payload (`POST /api/auth/register`):
```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "securepassword123",
  "confirmPassword": "securepassword123"
}
```

---

### Task Endpoints

| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/tasks` | Get all user tasks (supports `search`, `status`, `priority`, `sortBy`) | ✅ Yes |
| `GET` | `/api/tasks/stats/summary` | Get task status statistics (Total, Pending, In-Progress, Completed) | ✅ Yes |
| `GET` | `/api/tasks/:id` | Get single task details | ✅ Yes |
| `POST` | `/api/tasks` | Create a new task | ✅ Yes |
| `PUT` | `/api/tasks/:id` | Update task details or status | ✅ Yes |
| `DELETE` | `/api/tasks/:id` | Delete a task | ✅ Yes |

#### Sample Task Payload (`POST /api/tasks`):
```json
{
  "title": "Complete System Integration Testing",
  "description": "Verify all REST API endpoints using Postman and frontend UI",
  "priority": "High",
  "status": "In Progress",
  "due_date": "2026-10-15"
}
```

---

## 🧪 API Testing with Postman

1. **Register User**: Send a `POST` request to `http://localhost:5000/api/auth/register`. Copy the token returned in `data.token`.
2. **Set Authorization Header**: For all task requests (`/api/tasks`), add an HTTP Header:
   - `Authorization`: `Bearer <YOUR_JWT_TOKEN>`
3. **Test Task CRUD**: Test creating, listing, filtering, updating, and deleting tasks.

---

## 🔮 Future Improvements

- 📧 Email notifications for approaching task due dates.
- 🏷️ Custom tag/category system for tasks.
- 👥 Team collaboration & shared tasks.
