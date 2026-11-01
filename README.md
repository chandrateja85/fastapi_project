# fastapi_project
# FastAPI Practice Project

A beginner-friendly project to learn **FastAPI, REST APIs, Pydantic, CRUD operations, JSON data handling, MySQL integration, and JWT-based authentication**.

This repository contains practical examples covering the fundamentals of backend API development using Python and FastAPI.

## 1. Technologies Used

- Python
- FastAPI
- Pydantic
- Uvicorn
- JSON
- SQLAlchemy
- MySQL
- MySQL Workbench
- JWT Authentication
- Argon2 Password Hashing

## 2. Project Structure

```text
fastapi_practice/
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
├── venv/
├── .gitignore
├── descriptions.py
├── main.py
├── patients.json
├── post.py
├── pydantic_delete.py
├── pydantic_patch.py
├── pydantic_post.py
├── pydantic_put.py
├── requirements.txt
└── README.md
```

## 3. Python Installation and Environment Setup

### Step 1: Install Python

Download Python from the official website:

https://www.python.org/downloads/

During installation, enable **Add Python to PATH**.

Verify the installation:

```powershell
python --version
```

### Step 2: Navigate to the project directory

Open the terminal inside your project folder:

```powershell
cd path\to\fastapi_practice
```

### Step 3: Create a virtual environment

If you don't already have one:

```powershell
python -m venv venv
```

### Step 4: Activate the virtual environment

For Windows PowerShell:

```powershell
.\venv\Scripts\Activate.ps1
```

If you already have a virtual environment, simply activate it.

### Step 5: Install dependencies

Install all required packages from the existing requirements file:

```powershell
pip install -r requirements.txt
```

## 4. Running FastAPI Applications

The project contains multiple independent FastAPI examples.

Run the required Python file using Uvicorn.

For example, to run the main application:

```powershell
uvicorn main:app --reload
```

To run another example:

```powershell
uvicorn descriptions:app --reload
```

The general command format is:

```powershell
uvicorn filename:app --reload
```

Here:
- `filename` is the Python file name without `.py`.
- `app` is the FastAPI application instance.
- `--reload` automatically restarts the server when code changes.

## 5. FastAPI Documentation – Swagger UI

Once the server starts, open:

**Swagger UI:**

http://127.0.0.1:8000/docs

**ReDoc:**

http://127.0.0.1:8000/redoc

Swagger UI allows you to explore endpoints, provide request parameters, send API requests, and view responses.

---

## 6. FastAPI Fundamentals and HTTP Methods

The project covers the following fundamental HTTP methods.

### GET – Retrieve Data

GET is used to retrieve information from the server.

Examples from the basic patient API:

| Endpoint | Description |
|---|---|
| `GET /` | Returns a welcome message |
| `GET /home` | Returns the home response |
| `GET /about` | Returns information about the application |
| `GET /contactus` | Returns contact information |
| `GET /patients` | Retrieves all patients |
| `GET /patients/{patient_id}` | Retrieves a patient using their ID |
| `GET /patients?name=Akhil` | Filters patients using their name |
| `GET /patients?disease=fever` | Filters patients using their disease |

Example:

```text
GET /patients/1
```

Response:

```json
{
  "id": 1,
  "name": "Akhil",
  "disease": "fever"
}
```

### POST – Create Data

POST is used to create new resources on the server.

In this project, `post.py` and `pydantic_post.py` demonstrate POST-related API development.

Typical operations include:

- Accepting request body data.
- Validating incoming data.
- Creating new records.
- Returning the created resource.

Example request:

```json
{
  "id": 4,
  "name": "Rahul",
  "disease": "Cold"
}
```

### PUT – Update Data

PUT is used to update an existing resource.

The `pydantic_put.py` file demonstrates PUT-based update operations.

Typical operations include:

- Accepting the resource ID.
- Receiving updated information.
- Validating the request data.
- Updating the existing record.

### PATCH – Partial Update

PATCH is used to update only selected fields of an existing resource.

The `pydantic_patch.py` file demonstrates partial update operations.

For example, updating only the disease field without modifying the patient's name.

### DELETE – Delete Data

DELETE is used to remove an existing resource.

The `pydantic_delete.py` file demonstrates DELETE-based operations.

Typical operations include:

- Accepting the resource ID.
- Finding the requested record.
- Removing the record.
- Returning a deletion confirmation.

---

## 7. Pydantic – Data Validation

Pydantic is used with FastAPI to validate and manage request and response data.

The project includes practical examples of Pydantic-based API development.

Important concepts covered:

- Defining Pydantic models using `BaseModel`.
- Field types and type validation.
- Required and optional fields.
- Request body validation.
- Data validation for POST requests.
- PUT and PATCH request validation.
- Handling invalid request data.

Example:

```python
from pydantic import BaseModel

class Patient(BaseModel):
    id: int
    name: str
    disease: str
```

FastAPI automatically validates incoming request data based on the defined Pydantic model.

If the request data does not match the expected types or validation rules, FastAPI returns a `422 Unprocessable Entity` response.

---

## 8. JSON Data Handling

The project also demonstrates working with JSON data.

The `patients.json` file contains patient-related sample data.

JSON is commonly used to exchange structured information between clients and backend APIs.

Example:

```json
[
  {
    "id": 1,
    "name": "Akhil",
    "disease": "fever"
  },
  {
    "id": 2,
    "name": "Bharat",
    "disease": "Cough"
  }
]
```

---

## 9. Authentication and Authorization Module

The `auth/` directory contains a separate authentication implementation using FastAPI.

It demonstrates:

- User registration.
- Password hashing using Argon2.
- User login.
- JWT access token generation.
- JWT token validation.
- Protected API endpoints.
- Role-based authorization.
- MySQL database integration using SQLAlchemy.

### Authentication Endpoints

| Method | Endpoint | Description |
|---|---|---|
| POST | `/signup` | Register a new user |
| POST | `/login` | Authenticate user and generate JWT |
| GET | `/protected` | Access a protected endpoint |
| GET | `/profile` | Access profile with permitted roles |
| GET | `/user/dashboard` | User-only dashboard |
| GET | `/admin/dashboard` | Admin-only dashboard |

### Running the Authentication Module

First, activate the virtual environment from the main project directory.

Navigate to the authentication folder:

```powershell
cd auth
```

Install dependencies if they are not already installed:

```powershell
pip install -r ..\requirements.txt
```

Configure the `.env` file inside `auth/` with your MySQL credentials.

Example:

```env
MYSQL_USER=root
MYSQL_PASSWORD=your_mysql_password
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_DATABASE=fastapi_db
```

### MySQL Database Setup

1. Install MySQL Server and MySQL Workbench.
2. Open MySQL Workbench.
3. Connect to your local MySQL Server.
4. Open a new SQL query tab.
5. Execute:

```sql
CREATE DATABASE fastapi_db;
```

6. Verify:

```sql
SHOW DATABASES;
```

The required tables are created automatically through SQLAlchemy when the authentication application runs, provided the database connection is configured correctly.

### Start the Authentication Application

From inside the `auth` directory:

```powershell
uvicorn main:app --reload --port 8001
```

Open the authentication Swagger documentation:

http://127.0.0.1:8001/docs

For more detailed authentication setup and explanations, refer to:

[`auth/README.md`](auth/README.md)

---

## 10. HTTP Status Codes

Some commonly used HTTP status codes:

| Status Code | Description |
|---|---|
| 200 | OK – Request successful |
| 201 | Created – Resource created successfully |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Validation Error |
| 500 | Internal Server Error |

---

## 11. Useful Commands

| Task | Command |
|---|---|
| Check Python version | `python --version` |
| Create virtual environment | `python -m venv venv` |
| Activate environment | `.\venv\Scripts\Activate.ps1` |
| Install dependencies | `pip install -r requirements.txt` |
| Run FastAPI | `uvicorn main:app --reload` |
| Open Swagger UI | `http://127.0.0.1:8000/docs` |
| Open ReDoc | `http://127.0.0.1:8000/redoc` |
| Deactivate environment | `deactivate` |

---

## 12. Learning Objectives

By working through this project, you will gain practical knowledge of:

- FastAPI application development.
- REST API fundamentals.
- GET, POST, PUT, PATCH, and DELETE methods.
- Path parameters and query parameters.
- Request body handling.
- Pydantic models and validation.
- JSON data handling.
- Database connectivity using SQLAlchemy.
- MySQL database management.
- User authentication using JWT.
- Password hashing and verification.
- Protected endpoints and role-based authorization.

## Conclusion

This repository serves as a hands-on learning resource for understanding FastAPI and backend API development, starting from basic endpoints and progressing towards Pydantic validation, CRUD operations, database integration, and JWT-based authentication.