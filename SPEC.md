### **SPEC.md**
**Project Name:** Tutorsly.org - Online Tutoring and Course Management Platform  
**Version:** 1.0  
**Date:** December 18, 2024  
**Author(s):** Vladimir Matkovskii

---

### **1. Overview**
Tutorsly.org is a scalable, high-performance platform connecting tutors with students. This document outlines the technical specifications for the backend implementation of Tutorsly.org, focusing on modularity, scalability, and testability. The backend will be developed using **Rust** and **Actix Web**, with **PostgreSQL** as the database.

---

### **2. Architecture**
#### **2.1. Overview of Modules**
The backend will follow a modular architecture with the following key components:
1. **Handlers**: Process HTTP requests and call appropriate services.
2. **Services (Business Logic)**: Contain core business logic.
3. **Database Access Layer**: Encapsulate all database interactions.
4. **Models**: Define the data structures used throughout the system.
5. **Routes**: Define API endpoints and link them to handlers.
6. **State**: Maintain shared application state, such as database connection pools.

#### **2.2. Components Diagram**
```plaintext
+-------------------+
|   HTTP Requests   |
+---------+---------+
          |
+---------v---------+
|      Routes       |
+---------+---------+
          |
+---------v---------+
|     Handlers      |
+---------+---------+
          |
+---------v---------+
|     Services      |
+---------+---------+
          |
+---------v---------+
| Database Access   |
+---------+---------+
          |
+---------v---------+
|      Database     |
+-------------------+
```

---

### **3. API Endpoints**
#### **3.1. Tutor Endpoints**
1. **Create a Tutor**
    - **Method**: POST
    - **Endpoint**: `/tutors`
    - **Request Body** (JSON):
      ```json
      {
        "name": "John Doe",
        "expertise": "Mathematics",
        "contact": "johndoe@example.com"
      }
      ```  
    - **Response** (201 Created):
      ```json
      {
        "id": 1,
        "name": "John Doe",
        "expertise": "Mathematics",
        "contact": "johndoe@example.com"
      }
      ```

2. **Get Tutor by ID**
    - **Method**: GET
    - **Endpoint**: `/tutors/{id}`
    - **Response** (200 OK):
      ```json
      {
        "id": 1,
        "name": "John Doe",
        "expertise": "Mathematics",
        "contact": "johndoe@example.com"
      }
      ```

#### **3.2. Course Endpoints**
1. **Create a Course**
    - **Method**: POST
    - **Endpoint**: `/courses`
    - **Request Body** (JSON):
      ```json
      {
        "title": "Algebra Basics",
        "description": "A course on basic algebra.",
        "category": "Mathematics",
        "tutor_id": 1
      }
      ```  
    - **Response** (201 Created):
      ```json
      {
        "id": 1,
        "title": "Algebra Basics",
        "description": "A course on basic algebra.",
        "category": "Mathematics",
        "tutor_id": 1
      }
      ```

2. **Get All Courses**
    - **Method**: GET
    - **Endpoint**: `/courses`
    - **Response** (200 OK):
      ```json
      [
        {
          "id": 1,
          "title": "Algebra Basics",
          "description": "A course on basic algebra.",
          "category": "Mathematics",
          "tutor_id": 1
        }
      ]
      ```

3. **Search Courses**
    - **Method**: GET
    - **Endpoint**: `/courses/search?query=algebra`
    - **Response** (200 OK):
      ```json
      [
        {
          "id": 1,
          "title": "Algebra Basics",
          "description": "A course on basic algebra.",
          "category": "Mathematics",
          "tutor_id": 1
        }
      ]
      ```

---

### **4. Database Schema**
#### **4.1. Tables**
1. **Tutors**
   ```sql
   CREATE TABLE tutors (
       id SERIAL PRIMARY KEY,
       name TEXT NOT NULL,
       expertise TEXT NOT NULL,
       contact TEXT NOT NULL UNIQUE
   );
   ```

2. **Courses**
   ```sql
   CREATE TABLE courses (
       id SERIAL PRIMARY KEY,
       title TEXT NOT NULL,
       description TEXT NOT NULL,
       category TEXT NOT NULL,
       tutor_id INT REFERENCES tutors(id)
   );
   ```

---

### **5. Environment Variables**
Define configuration in `.env` file:
```plaintext
DATABASE_URL=postgresql://user:password@localhost/tutorsly
SERVER_PORT=8080
RUST_LOG=info
```

---

### **6. Error Handling**
All errors will be converted into structured JSON responses:
1. **Validation Error**
    - **Status**: 400 Bad Request
    - **Response**:
      ```json
      {
        "error": "Invalid input",
        "details": "Field 'name' is required"
      }
      ```

2. **Resource Not Found**
    - **Status**: 404 Not Found
    - **Response**:
      ```json
      {
        "error": "Resource not found",
        "details": "Tutor with ID 99 not found"
      }
      ```

---

### **7. Performance Metrics**
- API response time must be under **200ms** for 95% of requests.
- The system should support **1000 concurrent users**.

---

### **8. Testing Plan**
#### **8.1. Unit Tests**
- Test individual modules (handlers, services, database access).
#### **8.2. Integration Tests**
- Test full API flows using integration tests in the `/tests` directory.
#### **8.3. Database Tests**
- Use a test database to validate schema and query performance.

---

This **SPEC.md** provides a comprehensive technical foundation for **Tutorsly.org** and can be iteratively refined as development progresses. Let me know if you want to expand any sections further! 🚀