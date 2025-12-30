# User Management System - Backend

## 1. Project Overview & Purpose

The **User Management System Backend** is a production-ready REST API built with **Flask**, designed to facilitate secure user authentication, role-based access control (RBAC), and profile management. It is architected to be scalable, secure, and easy to deploy.

**Key Features:**
*   **Authentication**: JWT-based stateless authentication with secure password hashing (bcrypt).
*   **Authorization**: Role-based permissions (Admin vs. Regular User) with decorator-based route protection.
*   **Database**: robust data persistence using SQLAlchemy ORM (SQLite for dev, PostgreSQL compatible for prod).
*   **Maintainability**: Modular application structure with Blueprints.

---

## 2. Tech Stack Used

| Layer | Technology | Description |
| :--- | :--- | :--- |
| **Framework** | **Flask** | Lightweight WSGI web application framework |
| **ORM** | **SQLAlchemy** | SQL Toolkit and Object Relational Mapper |
| **Auth** | **PyJWT** | JSON Web Token implementation for Python |
| **Security** | **Bcrypt** | Password hashing function |
| **Server** | **Gunicorn** | Python WSGI HTTP Server for UNIX |
| **DevOps** | **Docker** | Containerization for consistent environments |

---

## 3. Setup Instructions (Backend)

### Prerequisites
- Python 3.9+ installed
- Docker & Docker Compose (Optional but recommended)

### Option A: Via Docker (Recommended)
1.  **Clone the repository:**
    ```bash
    git clone https://github.com/akashcodejames/user-management-backend.git
    cd user-management-backend
    ```
2.  **Build and Run:**
    ```bash
    docker build -t user-backend .
    docker run -p 5001:5000 --env-file .env user-backend
    ```

### Option B: Local Python Environment
1.  **Create Virtual Environment:**
    ```bash
    python -m venv venv
    source venv/bin/activate  # Windows: venv\Scripts\activate
    ```
2.  **Install Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
3.  **Run Migrations:**
    ```bash
    flask db upgrade
    ```
4.  **Create Admin User (Optional):**
    ```bash
    python create_admin.py
    ```
5.  **Start Server:**
    ```bash
    python run.py
    ```
    The server will start at `http://localhost:5001`.

---

## 4. Environment Variables

Create a `.env` file in the root directory. **Do not commit this file.**

| Variable | Description |
| :--- | :--- |
| `FLASK_APP` | Application entry point (e.g., `run.py`) |
| `FLASK_ENV` | `development` or `production` |
| `DATABASE_URL` | DB connection string (e.g., `sqlite:////app/instance/app.db`) |
| `SECRET_KEY` | Secret key for Flask sessions and security |
| `JWT_SECRET_KEY` | Secret key for signing JSON Web Tokens |
| `CORS_ORIGINS` | Comma-separated list of allowed frontend origins (e.g., `http://localhost:5173`) |

---

## 5. Deployment Instructions

This application is deployment-ready for platforms like **Render**, **Railway**, or **AWS Elastic Beanstalk**.

### Deployment Steps (e.g., Render)
1.  **Push to GitHub**.
2.  **Create Web Service** on Render connected to your repo.
3.  **Configuration**:
    *   **Runtime**: Python 3
    *   **Build Command**: `pip install -r requirements.txt`
    *   **Start Command**: `gunicorn run:app`
4.  **Environment Variables**: Add the secrets from your local `.env`.
5.  **Database**: Provision a PostgreSQL instance (e.g., Render Postgres) and set `DATABASE_URL` to the internal connection string.

---

## 6. API Documentation

### Base URL: `/api`

### 1. **Login**
**Endpoint**: `POST /auth/login`

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "Password123!"
}
```

**Response (200 OK):**
```json
{
  "token": "eyJhbGciOiJIUzI1Ni...",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "role": "user"
  }
}
```

### 2. **Signup**
**Endpoint**: `POST /auth/signup`

**Request Body:**
```json
{
  "email": "newuser@example.com",
  "password": "Password123!",
  "full_name": "New User"
}
```

**Response (201 Created):**
```json
{
  "message": "User created successfully",
  "token": "eyJhbG..."
}
```

### 3. **Get Profile**
**Endpoint**: `GET /users/profile`  
**Headers**: `Authorization: Bearer <token>`

**Response (200 OK):**
```json
{
  "id": 1,
  "full_name": "Test User",
  "email": "user@example.com",
  "role": "user",
  "created_at": "2025-12-30T10:00:00"
}
```

### 4. **Admin: List Users**
**Endpoint**: `GET /admin/users?page=1&per_page=10`  
**Headers**: `Authorization: Bearer <token>` (Admin Role Required)

**Response (200 OK):**
```json
{
  "users": [
    {
      "id": 2,
      "email": "another@example.com",
      "status": "active"
    }
  ],
  "total": 5,
  "pages": 1
}
```
