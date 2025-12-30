# User Management System - Backend

## 1. Project Overview & Purpose

The **User Management System Backend** is a robust REST API built with **Flask**, designed to handle secure user authentication, role-based access control, and profile management. It serves as the core logic layer for the User Management System.

**Purpose:**
To provide a secure, scalable, and production-ready API that handles:
*   **Authentication**: JWT-based login/signup.
*   **Authorization**: Admin vs. User role permissions.
*   **Data Persistence**: SQLAlchemy ORM with SQLite (Dev) / PostgreSQL (Prod).

---

## 2. Tech Stack Used

| Technology | Usage |
| :--- | :--- |
| **Flask** | Web Framework |
| **SQLAlchemy** | ORM for database interactions |
| **PyJWT** | JSON Web Token authentication |
| **Flask-Migrate** | Database migrations (Alembic) |
| **Gunicorn** | WSGI HTTP Server for production |
| **Docker** | Containerization |

---

## 3. Setup Instructions (Backend)

### Prerequisites
- Python 3.9+
- Docker (Optional)

### Option A: Docker (Recommended)
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

### Option B: Local Python
1.  **Create Virtual Environment:**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: venv\Scripts\activate
    ```
2.  **Install Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
3.  **Run Migrations:**
    ```bash
    flask db upgrade
    ```
4.  **Start Server:**
    ```bash
    python run.py
    ```
    Server runs at `http://localhost:5001`.

---

## 4. Environment Variables

Create a `.env` file in the root directory.

| Variable | Description | Example (Do not use in prod) |
| :--- | :--- | :--- |
| `FLASK_APP` | Entry point | `run.py` |
| `FLASK_ENV` | Environment | `development` |
| `DATABASE_URL` | DB Connection | `sqlite:////app/instance/app.db` |
| `SECRET_KEY` | Session Secret | `dev-secret-key` |
| `JWT_SECRET_KEY` | JWT Secret | `dev-jwt-secret` |
| `CORS_ORIGINS` | Frontend URL | `http://localhost:5173` |

---

## 5. Deployment Instructions

This backend is ready for deployment on platforms like **Render**, **Railway**, or **AWS**.

1.  **Push to GitHub**.
2.  **Connect to Platform** (e.g., Render).
3.  **Settings**:
    *   **Build Command**: `pip install -r requirements.txt`
    *   **Start Command**: `gunicorn run:app`
4.  **Environment Variables**: Add the variables listed above in the platform's dashboard.

---

## 6. API Documentation

### Base URL: `/api`

#### Authentication

*   **`POST /auth/signup`**
    *   **Body**: `{"email": "...", "password": "...", "full_name": "..."}`
    *   **Response**: `201 Created`

*   **`POST /auth/login`**
    *   **Body**: `{"email": "...", "password": "..."}`
    *   **Response**: `200 OK` + `token`

#### Users

*   **`GET /users/profile`** (Auth Required)
    *   **Response**: User details.

#### Admin

*   **`GET /admin/users`** (Admin Auth Required)
    *   **Query**: `?page=1&per_page=10`
    *   **Response**: List of users with pagination.

*   **`PUT /admin/users/<id>/activate`** (Admin Auth Required)
    *   **Response**: Activates user account.
