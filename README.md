Вот подробный пример **README.md** для проекта на основе вашей книги "Rust Servers, Services, and Apps" с использованием **Actix** и **PostgreSQL**. Файл представляет структуру, описывает функционал и объясняет как запустить проект.

---

# 📚 Tutorsly Web Service in Rust

A RESTful web service built with Rust, leveraging the Actix Web framework and PostgreSQL for persistent data storage. This project is an implementation based on the book *"Rust Servers, Services, and Apps"* by Prabhu Eshwarla.

## 🚀 Features

- **RESTful APIs** to create and retrieve courses for tutors.
- **Asynchronous database access** using `SQLx` with PostgreSQL.
- Safe and efficient concurrency using Actix.
- In-memory application state sharing between handlers.
- Unit tests for API routes and handlers.

---

## 📂 Project Structure

```plaintext

tutorsly/                     # Root workspace for the project
├── PRD.md                    # Product document describing project goals and objectives
├── SPEC.md                   # Technical specification of the project
├── LICENSE.md                # Project LICENSE
├── README.md                 # General project description
├── Cargo.toml                # Workspace configuration file
├── .env                      # Environment variables (not added to Git)
├── src/                      # General folder containing application logic
│   ├── bin/                  # Main entry points for different iterations
│   │   ├── iter5.rs          # Final main application entry point
│   │   ├── iter1.rs          # Test implementation from Chapter 1
│   ├── iter5/                # Final application implementation
│   │   ├── dbaccess/         # Database access methods
│   │   │   ├── course.rs     # Database operations for the courses table
│   │   │   ├── tutor.rs      # Database operations for the tutors table
│   │   │   ├── mod.rs        # Module definition for dbaccess
│   │   ├── handlers/         # HTTP request handlers
│   │   │   ├── course.rs     # Handlers for course-related APIs
│   │   │   ├── tutor.rs      # Handlers for tutor-related APIs
│   │   │   ├── general.rs    # General handlers (e.g., health-check)
│   │   │   ├── mod.rs        # Module definition for handlers
│   │   ├── models/           # Data models for the application
│   │   │   ├── course.rs     # Data model for courses
│   │   │   ├── tutor.rs      # Data model for tutors
│   │   │   ├── mod.rs        # Module definition for models
│   │   ├── dbscripts/        # SQL scripts for database initialization
│   │   │   ├── course.sql    # Script to create the courses table
│   │   │   ├── tutor.sql     # Script to create the tutors table
│   │   ├── routes.rs         # API route definitions
│   │   ├── state.rs          # Shared application state
│   │   ├── errors.rs         # Error handling definitions
├── tutor-nodb/               # In-memory data storage implementation (for earlier chapters)
│   ├── src/
│   │   ├── main.rs           # Main service logic
│   │   ├── handlers.rs       # HTTP request handlers
│   │   ├── models.rs         # Data models for in-memory storage
│   │   ├── routes.rs         # API route definitions
│   │   └── state.rs          # Shared application state
│   ├── tests/                # Unit tests for the in-memory implementation
│   ├── Cargo.toml            # Configuration for this subproject
├── tutor-db/                 # Database-backed implementation
│   ├── src/
│   │   ├── main.rs           # Main service logic
│   │   ├── handlers.rs       # HTTP request handlers
│   │   ├── models.rs         # Data models for the database
│   │   ├── routes.rs         # API route definitions
│   │   └── state.rs          # Shared application state
│   ├── migrations/           # Database migration scripts
│   │   ├── V1__create_course_table.sql
│   │   ├── V2__add_tutor_table.sql
│   ├── tests/                # Unit tests for the database implementation
│   ├── .env                  # Database configuration
│   ├── Cargo.toml            # Configuration for this subproject
├── tests/                    # Integration tests for the overall project
│   ├── integration_tests.rs  # API integration tests


```

---

## ⚙️ Dependencies

- **Rust** 1.59.0 or later
- **Actix Web** 4.1.0 for building web servers.
- **SQLx** for async database operations.
- **PostgresSQL** as the relational database.
- **dotenv** for managing environment variables.

Install all dependencies using Cargo:
```bash
cargo check
```

---

## 🛠️ Setup Instructions

### 1. Install PostgreSQL
Make sure PostgreSQL is installed. Create a database and user:

```sql
CREATE DATABASE tutorsly;
CREATE USER truuser WITH PASSWORD 'mypassword';
GRANT ALL PRIVILEGES ON DATABASE tutorsly TO truuser;
```

### 2. Run the SQL Script
Set up the schema for the database:

```bash
psql -U truuser -d tutorsly -f src/database.sql
```

### 3. Set Environment Variables
Create a `.env` file in the project root:

```plaintext
DATABASE_URL=postgres://truuser:mypassword@127.0.0.1:5432/tutorsly
```

### 4. Run the Service
Build and start the web service:

```bash
cargo run --bin iter2
```

The service will be available at:
- **Health Check**: `http://localhost:3000/health`
- **POST /courses**: Add a new course.
- **GET /courses/{tutor_id}**: Get all courses for a tutor.
- **GET /courses/{tutor_id}/{course_id}**: Get course details.

---

## 🧪 Testing

Run the unit and integration tests:

```bash
cargo test
```

---

## 🛠️ API Endpoints

### 1. **Health Check**

- **Method**: `GET`
- **URL**: `/health`
- **Response**:
```json
"I'm good. You've already asked me N times"
```

### 2. **Create a New Course**

- **Method**: `POST`
- **URL**: `/courses/`
- **Request Body**:
```json
{
    "tutor_id": 1,
    "course_name": "Intro to Rust"
}
```
- **Response**:
```json
"Added course"
```

### 3. **Retrieve All Courses for a Tutor**

- **Method**: `GET`
- **URL**: `/courses/{tutor_id}`
- **Response**:
```json
[
    {
        "tutor_id": 1,
        "course_id": 1,
        "course_name": "Intro to Rust",
        "posted_time": "2024-06-14T10:00:00"
    }
]
```

### 4. **Retrieve a Single Course**

- **Method**: `GET`
- **URL**: `/courses/{tutor_id}/{course_id}`
- **Response**:
```json
{
    "tutor_id": 1,
    "course_id": 1,
    "course_name": "Intro to Rust",
    "posted_time": "2024-06-14T10:00:00"
}
```

---

## 🤝 Contributing

Feel free to submit a pull request or open an issue for discussions.

---

## 📜 License

This project is licensed under the MIT License.

---

Enjoy building efficient and scalable web services with Rust and Actix! 🚀