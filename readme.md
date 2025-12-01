# Automated Test Execution Framework

## 1. Introduction
The Automated Test Execution Framework is a backend application developed using Java and Spring Boot to manage, execute, and track software test cases in a centralized system. The project addresses the challenges of manual test case management using Excel or unorganized tools by providing a secure, scalable, and database-driven solution.

This framework allows QA engineers and project managers to store test cases, execute them, record execution results, and analyze historical performance effectively.

---

## 2. Objectives
- To provide a centralized system for test case management.
- To automate the process of test execution recording.
- To maintain execution history and generate reports.
- To ensure secure access using authentication and authorization.
- To enable future integration with automated testing tools like Selenium or JUnit.

---

## 3. Scope of the Project
The scope of this project is limited to backend development and API management. A frontend interface can be integrated in future versions. This framework is suitable for small to medium QA teams to manage regression, functional, and smoke testing cycles.

---

## 4. Technology Stack
- Programming Language: Java
- Framework: Spring Boot
- Build Tool: Maven
- Database: MySQL
- ORM: Spring Data JPA
- Security: Spring Security
- API Testing Tool: Postman
- Version Control: Git & GitHub

---

## 5. System Architecture
The project follows a layered architecture:

1. Presentation Layer (Controller)
2. Business Logic Layer (Service)
3. Data Access Layer (Repository)
4. Database Layer (MySQL)

Each layer is independent and communicates with adjacent layers through well-defined interfaces.

---

## 6. Module Description

### 6.1 User Management and Security Module
This module handles authentication and authorization using Spring Security:
- Secure login system
- Role-based access control
- Encrypted password storage

Entities Used:
- User
- Role

---

### 6.2 Test Case Management Module
This module allows users to perform CRUD operations on test cases.

Features:
- Create new test cases
- Update existing test cases
- View all test cases
- Delete test cases

Test Case Attributes:
- Test Case ID
- Title
- Description
- Preconditions
- Test Steps
- Expected Result
- Priority
- Module
- Status
- Created By
- Created Date

---

### 6.3 Test Execution Module
This module records execution details of test cases.

Features:
- Execute test cases
- Store execution status
- Maintain execution history

Test Execution Attributes:
- Execution ID
- Test Case ID
- Execution Time
- Status (PASSED, FAILED, BLOCKED, NOT_RUN)
- Executed By
- Remarks

---

### 6.4 Reporting Module
This module generates testing reports based on execution history:
- Pass/Fail summary
- Execution trends
- Module-wise defect analysis

Reports can be exported for further analysis.

---

## 7. Database Design

### 7.1 User Table
- user_id
- username
- password
- enabled

### 7.2 Role Table
- role_id
- role_name

### 7.3 Test Case Table
- test_case_id
- title
- description
- steps
- expected_result
- priority
- module
- status

### 7.4 Test Execution Table
- execution_id
- test_case_id (foreign key)
- execution_time
- status
- executed_by
- remarks

---

## 8. API Documentation (REST APIs)

This section describes all the REST APIs exposed by the Automated Test Execution Framework.

---

### 8.1 Authentication APIs

#### 8.1.1 POST /login
- **Description:** Authenticates a user and starts a secure session.
- **Method:** POST
- **URL:** `/login`
- **Headers:**
  - `Content-Type: application/json`
- **Request Body (JSON):**
```json
{
  "username": "yourname",
  "password": "yourPassword"
}
```
- **Success Response:**
  - **Status:** `200 OK`
  - **Body:**
    - Depends on configuration. Typically returns authentication success message or redirects/sets session.
- **Error Responses:**
  - `401 UNAUTHORIZED` – invalid username or password.

---

### 8.2 Test Case APIs

These APIs are used to manage test cases.

#### 8.2.1 Create Test Case
- **Method:** POST
- **URL:** `/api/test-cases`
- **Description:** Creates a new test case.
- **Headers:**
  - `Content-Type: application/json`
  - `Authorization: Bearer <token>` or secured session (depending on configuration)
- **Request Body (JSON):**
```json
{
  "title": "Verify Login with Valid Credentials",
  "description": "Check that user can log in using valid credentials",
  "preconditions": "User account exists",
  "steps": "1. Open login page 2. Enter valid username/password 3. Click Login",
  "expectedResult": "User is redirected to dashboard",
  "priority": "HIGH",
  "module": "Authentication",
  "status": "ACTIVE"
}
```
- **Success Response:**
  - **Status:** `201 CREATED`
  - **Body:** JSON of created test case with generated `testCaseId`.
- **Error Responses:**
  - `400 BAD REQUEST` – missing or invalid fields.
  - `401 UNAUTHORIZED` – not logged in.

#### 8.2.2 Get All Test Cases
- **Method:** GET
- **URL:** `/api/test-cases`
- **Description:** Returns a list of all test cases.
- **Headers:**
  - `Authorization: Bearer <token>`
- **Success Response:**
  - **Status:** `200 OK`
  - **Body (JSON Array):**
```json
[
  {
    "testCaseId": 1,
    "title": "Verify Login with Valid Credentials",
    "priority": "HIGH",
    "module": "Authentication",
    "status": "ACTIVE"
  },
  {
    "testCaseId": 2,
    "title": "Verify Login with Invalid Password",
    "priority": "MEDIUM",
    "module": "Authentication",
    "status": "ACTIVE"
  }
]
```

#### 8.2.3 Get Test Case by ID
- **Method:** GET
- **URL:** `/api/test-cases/{id}`
- **Description:** Fetches details of a single test case by its ID.
- **Path Variable:** `id` – numeric ID of the test case.
- **Success Response:**
  - **Status:** `200 OK`
  - **Body:** Full JSON details of the test case.
- **Error Responses:**
  - `404 NOT FOUND` – if test case does not exist.

#### 8.2.4 Update Test Case
- **Method:** PUT
- **URL:** `/api/test-cases/{id}`
- **Description:** Updates an existing test case.
- **Request Body (JSON):** same structure as create.
- **Success Response:**
  - **Status:** `200 OK`
  - **Body:** Updated test case JSON.

#### 8.2.5 Delete Test Case
- **Method:** DELETE
- **URL:** `/api/test-cases/{id}`
- **Description:** Deletes or deactivates a test case.
- **Success Response:**
  - **Status:** `200 OK` or `204 NO CONTENT`.
- **Error Responses:**
  - `404 NOT FOUND` – if test case does not exist.

---

### 8.3 Test Execution APIs

These APIs are used to execute test cases and view execution results.

#### 8.3.1 Execute Test Case
- **Method:** POST
- **URL:** `/api/test-executions`
- **Description:** Creates a new execution record for a test case.
- **Headers:**
  - `Content-Type: application/json`
  - `Authorization: Bearer <token>`
- **Request Body (JSON):**
```json
{
  "testCaseId": 1,
  "environment": "QA",
  "executedBy": "yourname",
  "status": "PASSED",
  "remarks": "All validations passed"
}
```
- **Success Response:**
  - **Status:** `201 CREATED`
  - **Body:**
```json
{
  "executionId": 101,
  "testCaseId": 1,
  "status": "PASSED",
  "executionTime": "2025-12-01T10:45:30",
  "executedBy": "yourname",
  "remarks": "All validations passed"
}
```

#### 8.3.2 Get All Executions
- **Method:** GET
- **URL:** `/api/test-executions`
- **Description:** Returns all test execution records.
- **Success Response:**
  - **Status:** `200 OK`
  - **Body:** Array of execution objects.

#### 8.3.3 Get Execution by ID
- **Method:** GET
- **URL:** `/api/test-executions/{id}`
- **Description:** Returns a single execution record by ID.
- **Success Response:**
  - **Status:** `200 OK`
- **Error Responses:**
  - `404 NOT FOUND` – if execution ID does not exist.

#### 8.3.4 Get Executions of a Test Case
- **Method:** GET
- **URL:** `/api/test-cases/{id}/executions`
- **Description:** Returns the execution history of a particular test case.
- **Path Variable:** `id` – test case ID.
- **Success Response:**
  - **Status:** `200 OK`
  - **Body (JSON Array):** list of executions linked to the given test case.

---

## 9. Input and Output Format

### Create Test Case Input (JSON)
{
  "title": "Verify Login",
  "description": "Check login with valid credentials",
  "steps": "Enter username and password",
  "expectedResult": "User should be logged in",
  "priority": "HIGH",
  "module": "Authentication"
}

### Test Execution Output (JSON)
{
  "executionId": 101,
  "testCaseId": 1,
  "status": "PASSED",
  "executionTime": "2025-12-01T10:45:30"
}

---

## 10. End-to-End Workflow


 Input and Output Format

### Create Test Case Input (JSON)
{
  "title": "Verify Login",
  "description": "Check login with valid credentials",
  "steps": "Enter username and password",
  "expectedResult": "User should be logged in",
  "priority": "HIGH",
  "module": "Authentication"
}

### Test Execution Output (JSON)
{
  "executionId": 101,
  "testCaseId": 1,
  "status": "PASSED",
  "executionTime": "2025-12-01T10:45:30"
}

---

## 10. End-to-End Workflow
1. User logs into the system.
2. User creates test cases.
3. User executes test cases.
4. System stores execution results.
5. User views execution history and reports.

---

## 11. OOPS Concepts Used
- Encapsulation
- Abstraction
- Inheritance
- Polymorphism

---

## 12. Advantages of the System
- Centralized test data management
- Reduced manual workload
- Secure and scalable
- Accurate execution history
- Easy API integration

---

## 13. Limitations
- No UI frontend available
- Manual result update (if automation not integrated)
- Limited reporting visualization

---

## 14. Future Enhancements
- Integration with Selenium and JUnit
- CI/CD pipeline integration
- Web UI using React or Angular
- Real-time email notifications
- Advanced analytics dashboard

---

## 15. Conclusion
The Automated Test Execution Framework simplifies the entire software testing lifecycle from test case creation to execution tracking and reporting. It provides a secure, efficient, and extensible backend solution that can be easily enhanced with automation tools and UI dashboards in the future.

