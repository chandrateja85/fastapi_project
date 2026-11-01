# FastAPI Authentication Module

## 1. Introduction

This project demonstrates how to implement an authentication and authorization system using **FastAPI, Python, MySQL, SQLAlchemy, password hashing, and JWT (JSON Web Tokens)**.

The module allows users to:

- Register a new account.
- Store passwords securely using password hashing.
- Log in using username and password.
- Generate JWT access tokens after successful login.
- Access protected API endpoints using JWT tokens.
- Restrict access to specific endpoints based on user roles.

The objective is to understand how authentication and authorization work in a real-world backend application.

---

## 2. Authentication vs Authorization

### Authentication

Authentication is the process of verifying the identity of a user.

It answers the question:

**Who are you?**

For example:
- A user enters their username and password.
- The application verifies the credentials.
- If the credentials are valid, the user is authenticated.

### Authorization

Authorization determines what an authenticated user is allowed to access.

It answers the question:

**What are you allowed to do?**

For example:
- A normal user can access the user dashboard.
- An admin can access the admin dashboard.
- A normal user cannot access admin-only endpoints.

### Authentication Flow

1. The user registers with a username, email, and password.
2. The application hashes the password and stores user information in MySQL.
3. The user logs in using their credentials.
4. The application verifies the password.
5. If the credentials are valid, a JWT access token is generated.
6. The client sends this token when accessing protected endpoints.
7. FastAPI validates the token and identifies the current user.
8. Role-based authorization determines whether the user can access the requested endpoint.

---

## 3. Technologies Used

| Technology | Purpose |
|---|---|
| Python | Backend programming language |
| FastAPI | Building REST APIs |
| MySQL | Relational database |
| MySQL Workbench | Database management and SQL interface |
| SQLAlchemy | ORM for database operations |
| Pydantic | Request data validation |
| Passlib | Password hashing and verification |
| Argon2 | Password hashing algorithm |
| JWT | Token-based authentication |
| Uvicorn | ASGI server |
| python-dotenv | Loading environment variables |

---

## 4. Project Structure

The authentication module is part of the main `fastapi_practice` project.

```text
fastapi_practice/
│
├── venv/
│
├── auth/
│   ├── .env
│   ├── auth_database.py
│   ├── auth_table.py
│   ├── key.py
│   ├── main.py
│   ├── models.py
│   ├── schemas.py
│   ├── utils.py
│   └── README.md
│
├── requirements.txt
└── other_fastapi_practice_files/
```

**Note:** The virtual environment is created in the main project directory, not inside the `auth` module.

### File Description

| File | Responsibility |
|---|---|
| `.env` | Stores database credentials |
| `auth_database.py` | Configures the SQLAlchemy database connection |
| `auth_table.py` | Creates database tables using SQLAlchemy metadata |
| `key.py` | Contains JWT secret-key generation logic |
| `main.py` | Contains API endpoints and authentication logic |
| `models.py` | Defines SQLAlchemy database models |
| `schemas.py` | Defines Pydantic request schemas |
| `utils.py` | Contains password hashing and verification functions |
| `requirements.txt` | Contains project dependencies |
| `README.md` | Project documentation |

---

## 5. Prerequisites

Before running the project, install:

1. Python 3.10 or above.
2. MySQL Community Server.
3. MySQL Workbench.
4. Visual Studio Code or another code editor.

### Installing MySQL Server and MySQL Workbench

MySQL Server and MySQL Workbench serve different purposes.

- **MySQL Server:** The actual database engine that stores and manages data.
- **MySQL Workbench:** A graphical interface used to connect to MySQL Server, execute SQL queries, and manage databases.

**Important:** Installing MySQL Workbench alone is not sufficient. You must also install MySQL Server.

Download the installer from:

https://dev.mysql.com/downloads/installer/

During installation:

1. Open the MySQL Installer.
2. Select MySQL Server and MySQL Workbench.
3. Install both components.
4. Configure the MySQL root username and password.
5. Keep the default MySQL port as `3306`, unless you have configured another port.
6. Complete the installation and ensure the MySQL Server service is running.

Remember the root password. You will need it when configuring the FastAPI application.

---

## 6. Setting Up the Python Virtual Environment

### Step 1: Open the main project directory

Open your terminal in the main `fastapi_practice` directory.

For example:

```powershell
cd path\to\fastapi_practice
```

Replace this example path with the actual location of your project.

### Step 2: Create a virtual environment

If you do not already have a virtual environment:

```powershell
python -m venv venv
```

This creates an isolated Python environment named `venv` in the main project directory.

### Step 3: Activate the virtual environment

For Windows PowerShell:

```powershell
.\venv\Scripts\Activate.ps1
```

After activation, your terminal should display `(venv)`.

### If you already have a virtual environment

You do not need to create another one.

Simply activate your existing environment:

```powershell
.\venv\Scripts\Activate.ps1
```

### Step 4: Navigate to the authentication module

After activating the virtual environment:

```powershell
cd auth
```

### Step 5: Install project dependencies

The project already contains a `requirements.txt` file.

Install all required packages using:

```powershell
pip install -r requirements.txt
```

This installs the dependencies listed in `requirements.txt` into your activated virtual environment.

**Important:** Make sure you activate the virtual environment from the main project directory before installing dependencies.

---

## 7. MySQL Database Configuration

### Step 1: Open MySQL Workbench

After installing MySQL Server and Workbench:

1. Open MySQL Workbench from the Windows Start menu.
2. You will see the MySQL Workbench home screen.
3. Under **MySQL Connections**, locate your local MySQL Server connection.
4. Double-click the connection, commonly named `Local instance MySQL80`.
5. Enter your MySQL root password.
6. Click OK to connect.

You should now see the MySQL Workbench SQL Editor.

### Step 2: Open a new SQL query tab

1. In the SQL Editor, click the **New SQL Tab** icon.
2. A blank query editor will open.
3. Enter the following SQL command:

```sql
CREATE DATABASE fastapi_db;
```

4. Click the lightning bolt icon to execute the query.

This creates a database named `fastapi_db`.

### Step 3: Verify database creation

Execute:

```sql
SHOW DATABASES;
```

You should see `fastapi_db` in the list of databases.

Alternatively, you can refresh the **Schemas** panel on the left side of MySQL Workbench to see the newly created database.

### Step 4: Select the database

You can select the database using:

```sql
USE fastapi_db;
```

**Important:** Only the database needs to be created manually. The application will create the required tables automatically using SQLAlchemy.

---

## 8. Configure Environment Variables Using `.env`

Inside the `auth` folder, create a file named `.env`.

Add the following database configuration:

```env
MYSQL_USER=root
MYSQL_PASSWORD=your_mysql_password
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_DATABASE=fastapi_db
```

Replace `your_mysql_password` with the password configured during MySQL Server installation.

For example:

```env
MYSQL_USER=root
MYSQL_PASSWORD=root123
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_DATABASE=fastapi_db
```

Do not add spaces around the `=` sign.

### Why do we use `.env`?

We use environment variables to keep database credentials and configuration separate from application code.

The application loads these values using `python-dotenv`.

Example:

```python
from dotenv import load_dotenv
import os

load_dotenv()

MYSQL_USER = os.getenv("MYSQL_USER")
MYSQL_PASSWORD = os.getenv("MYSQL_PASSWORD")
MYSQL_HOST = os.getenv("MYSQL_HOST")
MYSQL_PORT = os.getenv("MYSQL_PORT")
MYSQL_DATABASE = os.getenv("MYSQL_DATABASE")
```

### Important security practice

Never upload your `.env` file to GitHub because it contains sensitive database credentials.

Create a `.gitignore` file in the main project directory and add:

```text
venv/
__pycache__/
.env
*.pyc
```

This prevents Git from tracking the virtual environment, environment files, and Python cache files.

---

## 9. Database Connection and Table Creation

### auth_database.py

This file configures the SQLAlchemy database connection.

It contains:

- Database URL configuration.
- SQLAlchemy engine creation.
- Session factory.
- Database dependency.
- Declarative Base.

The database URL follows this structure:

```text
mysql+pymysql://username:password@host:port/database_name
```

The SQLAlchemy engine manages database connectivity, while `SessionLocal` creates database sessions.

The `get_db()` function provides a database session to API endpoints and closes it after the request is completed.

### models.py

This file defines the database structure using SQLAlchemy ORM.

The `User` model represents the users table.

It contains:

- `id`
- `username`
- `email`
- `hashed_password`
- `role`

### auth_table.py

This file creates the database tables using SQLAlchemy.

The following code is responsible for creating missing tables:

```python
Base.metadata.create_all(bind=engine)
```

Before calling `create_all()`, the model must be imported so SQLAlchemy knows which tables are registered with the Base metadata.

**Important:** `create_all()` creates missing tables. It does not automatically modify existing table structures when your model changes.

---

## 10. Understanding the Authentication Components

### schemas.py – Request Validation

Pydantic schemas validate incoming request data.

For example:

**UserCreate**
- username
- email
- password
- role

**UserLogin**
- username
- password

Schemas define the structure and validation rules for API request data.

### utils.py – Password Hashing

Passwords should never be stored as plain text in a database.

We use Argon2 to securely hash passwords.

Two important functions are:

```python
hash_password(password)
```

Converts a plain-text password into a secure password hash.

```python
verify_password(plain_password, hashed_password)
```

Checks whether the entered password matches the stored hash.

### main.py – API Endpoints

This file contains the application's API endpoints and authentication logic.

It implements:

- User registration.
- User login.
- JWT token generation.
- JWT token validation.
- Protected routes.
- Role-based access control.

### JWT – JSON Web Token

JWT is a token format used to transmit information between a client and server.

A JWT typically contains three parts:

1. Header – Specifies the token type and signing algorithm.
2. Payload – Contains claims such as username, role, and expiration time.
3. Signature – Used to verify that the token has not been modified.

In our project, we use the HS256 signing algorithm.

The token includes:

- `sub` – Username of the authenticated user.
- `role` – User role.
- `exp` – Token expiration time.

The access token expires after 30 minutes in the current implementation.

### OAuth2PasswordBearer

FastAPI provides `OAuth2PasswordBearer` to extract a bearer token from the Authorization header.

The client sends the token in this format:

```http
Authorization: Bearer <access_token>
```

The token is then passed to the dependency that validates it.

---

## 11. Running the Application

Before starting the application, make sure:

- MySQL Server is running.
- The `fastapi_db` database exists.
- Your `.env` file contains the correct database credentials.
- Your virtual environment is activated.
- All dependencies are installed.

If you are currently inside the `auth` directory, run:

```powershell
uvicorn main:app --reload
```

Here:

- `main` refers to the `main.py` file.
- `app` refers to the FastAPI application instance.
- `--reload` automatically restarts the server when code changes.

If the application starts successfully, you should see a message similar to:

```text
Uvicorn running on http://127.0.0.1:8000
```

The table-creation logic imported through `main.py` will run during application startup, provided the database connection and imports are configured correctly.

---

## 12. Accessing Swagger UI

FastAPI automatically generates interactive API documentation.

Open the following URL in your browser:

http://127.0.0.1:8000/docs

Swagger UI displays all available endpoints and allows you to execute API requests directly from the browser.

---

## 13. Testing the Authentication APIs

### 1. User Registration – POST `/signup`

Register a new user.

Example request body:

```json
{
  "username": "chandra",
  "email": "chandra@example.com",
  "password": "Test@123",
  "role": "user"
}
```

Expected result:

- The application checks whether the username already exists.
- The password is hashed using Argon2.
- User information is inserted into MySQL.
- The newly created user information is returned.

**Note:** The current demonstration accepts the role from the request. In a real application, public registration should assign the default `user` role on the server rather than allowing clients to register themselves as admins.

### 2. User Login – POST `/login`

Log in using the registered credentials.

This endpoint uses `OAuth2PasswordRequestForm`, so provide form data rather than a JSON request body.

Example:

```text
username: chandra
password: Test@123
```

If the credentials are valid, the API returns:

```json
{
  "access_token": "<generated_jwt_token>",
  "token_type": "bearer"
}
```

Copy the access token.

### 3. Protected Endpoint – GET `/protected`

This endpoint requires a valid JWT access token.

In Swagger UI:

1. Open the `/protected` endpoint.
2. Click **Try it out**.
3. Click **Execute**.
4. If required, use the Swagger **Authorize** option to provide the bearer token.

The endpoint returns a welcome message for the authenticated user.

### 4. Profile Endpoint – GET `/profile`

Accessible to:

- User
- Admin

Both roles are permitted to access this endpoint.

### 5. User Dashboard – GET `/user/dashboard`

Accessible only to users with the `user` role.

An admin token will not satisfy this endpoint's current role restriction.

### 6. Admin Dashboard – GET `/admin/dashboard`

Accessible only to users with the `admin` role.

A normal user will receive a `403 Forbidden` response.

---

## 14. Understanding HTTP Status Codes

| Status Code | Meaning |
|---|---|
| 200 | Request successful |
| 400 | Bad request, such as an existing username |
| 401 | Unauthorized – invalid or missing authentication credentials |
| 403 | Forbidden – authenticated user does not have permission |
| 422 | Request validation error |
| 500 | Internal server error |

### 401 vs 403

**401 Unauthorized:** The user has not provided valid authentication credentials.

Example: Invalid or expired JWT token.

**403 Forbidden:** The user is authenticated, but does not have the required permission.

Example: A normal user attempts to access an admin-only dashboard.

---

## 15. Complete Execution Flow

```text
                 Client
                   |
                   |
             POST /signup
                   |
                   v
          Validate User Details
                   |
                   v
          Hash the Password
                   |
                   v
             MySQL Database
                   |
                   |
             POST /login
                   |
                   v
         Verify Username/Password
                   |
                   v
          Generate JWT Token
                   |
                   v
             Return Token
                   |
                   v
       Client Sends Bearer Token
                   |
                   v
          Validate JWT Token
                   |
                   v
          Identify Current User
                   |
                   v
        Check User Role/Permission
                   |
             +-----+-----+
             |           |
          Allowed      Denied
             |           |
             v           v
        Return Data   403 Forbidden
```

---

## 16. Important Notes

- Never store plain-text passwords in the database.
- Never commit `.env` files or database passwords to GitHub.
- Keep MySQL Server running before starting the application.
- Create the database before running the application.
- The tables are created through SQLAlchemy code.
- Use Swagger UI to test the endpoints.
- Use `401` for authentication failures and `403` for authorization failures.
- For production applications, implement proper role assignment, token revocation/refresh strategies, database migrations, and robust exception handling.

---

## 17. Useful Commands

| Task | Command |
|---|---|
| Create virtual environment | `python -m venv venv` |
| Activate virtual environment | `.\venv\Scripts\Activate.ps1` |
| Install dependencies | `pip install -r requirements.txt` |
| Start FastAPI | `uvicorn main:app --reload` |
| Open Swagger UI | `http://127.0.0.1:8000/docs` |
| Deactivate virtual environment | `deactivate` |
| Generate requirements file | `pip freeze > requirements.txt` |

---

## Conclusion

This authentication module demonstrates the fundamentals of building a token-based authentication system with FastAPI.

By completing this module, you will understand how user registration, password hashing, database integration, JWT authentication, protected endpoints, and role-based authorization work together in a backend application.