# Spring Boot Library Management System

A Java-based library management application that combines a **Spring Boot REST API** with a terminal-based interface for managing books, users, and reservations. The project explores layered application architecture, database persistence, authentication and authorization, custom exception handling, and Google Calendar integration.

<p align="center">
  <img src="https://img.shields.io/badge/Java-Spring%20Boot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white" alt="Java Spring Boot" />
  <img src="https://img.shields.io/badge/REST-API-005571?style=for-the-badge" alt="REST API" />
  <img src="https://img.shields.io/badge/Database-H2%20%7C%20MongoDB-blue?style=for-the-badge" alt="H2 and MongoDB" />
  <img src="https://img.shields.io/badge/Integration-Google%20Calendar-4285F4?style=for-the-badge&logo=googlecalendar&logoColor=white" alt="Google Calendar" />
</p>

---

## Overview

The application provides two interfaces for interacting with the library system:

- **REST API:** Manage and retrieve users, books, and reservations through HTTP endpoints, with interactive documentation available through Swagger UI.
- **Terminal interface:** A command-line client with authentication and role-based menus for users, administrators, and managers.

The project also demonstrates persistence with H2 and MongoDB, validation, logging, aspect-oriented programming (AOP), unit testing, and integration with Google Calendar for reservations.

## Architecture

<p align="center">
  <img src="https://img.shields.io/badge/Client-REST%20API%20%7C%20Terminal-4C6E91?style=for-the-badge" alt="Client layer" />
  <br />↓<br />
  <img src="https://img.shields.io/badge/Application-Spring%20Boot-6DB33F?style=for-the-badge" alt="Application layer" />
  <br />↓<br />
  <img src="https://img.shields.io/badge/Business%20Logic-Services%20%26%20Validation-8A63D2?style=for-the-badge" alt="Business logic layer" />
  <br />↓<br />
  <img src="https://img.shields.io/badge/Persistence-JPA%20%7C%20Entity%20Manager%20%7C%20MongoDB-287A9F?style=for-the-badge" alt="Persistence layer" />
  <br />↓<br />
  <img src="https://img.shields.io/badge/External%20Integration-Google%20Calendar-4285F4?style=for-the-badge" alt="External integration" />
</p>

The system demonstrates a layered design that separates client interaction, application logic, and data access. Google Calendar provides an external integration for reservation-related events.

## Features

| Category | Features |
|---|---|
| REST API | Endpoints for users, books, and reservations |
| API documentation | Interactive Swagger UI |
| Architecture | Three-tier architecture and dependency injection |
| Data management | JPA, Entity Manager, H2, and MongoDB integration |
| Authentication | Login and password verification |
| Authorization | Role-based access for users, admins, and managers |
| Error handling | Custom exceptions and validation |
| Logging | SLF4J and AOP-based logging |
| Testing | Unit testing |
| External services | Google Calendar reservation synchronization |
| Terminal client | Interactive menus and library management operations |
| Legacy implementation | In-memory `ArrayList`-based data layer |

---

## REST API

Start the Spring Boot application and use the following endpoints to interact with the system.

### API documentation

Swagger UI provides an interactive interface for exploring and testing the available API operations.

<p align="center">
  <img src="https://img.shields.io/badge/Swagger-Open%20API%20Documentation-85EA2D?style=for-the-badge&logo=swagger&logoColor=black" alt="Swagger API documentation" />
</p>

**Local URL:** `http://localhost:8080/swagger-ui/index.html`

### Main endpoints

| Resource | Method | Endpoint | Purpose |
|---|---|---|---|
| Users | `GET` | `/users` | Retrieve users |
| Books | `GET` | `/books` | Retrieve books |
| Reservations | `GET` | `/reservations` | Retrieve reservations |
| User lookup | `GET` | `/users/20` | Demonstrate custom exception handling for a nonexistent user, if ID 20 is absent |
| Create book | `POST` | `/books` | Submit a new book |
| Reserve book | `POST` | `/users/13/reserve-book?bookName=DotinProject` | Request a reservation for a book |

For local testing, prepend `http://localhost:8080` to each endpoint.

### Example: Create a book

**Request**

```http
POST http://localhost:8080/books
Content-Type: application/json
```

**Request body**

```json
{
  "title": "DotinProject",
  "author": "Maryam",
  "available": true,
  "pages": 635,
  "user": null
}
```

This example submits a book with its title, author, availability, page count, and user reference.

### Example: Reserve a book

```http
POST http://localhost:8080/users/13/reserve-book?bookName=DotinProject
```

The reservation endpoint connects the user and book reservation workflow with the Google Calendar integration.

> **Note:** The example IDs and book name are sample values. Make sure the corresponding records exist in your local database before testing the request.

---

## Database

### H2 Console

The project uses an embedded H2 database for local persistence.

**Console URL:** `http://localhost:8080/h2-console`

**JDBC URL**

```text
jdbc:h2:file:./data/library_database
```

Use the JDBC URL above when configuring the H2 Console. The appropriate username and password depend on the application's database configuration.

The project also includes MongoDB integration, alongside JPA and Entity Manager usage. Check the application's configuration to determine which persistence implementation is active for each module.

---

## Google Calendar Integration

The application integrates with Google Calendar so that reservations can be synchronized with calendar events.

### Reservation workflow

<p align="center">
  <img src="https://img.shields.io/badge/1-Submit%20Reservation-4C6E91?style=for-the-badge" alt="Submit reservation" />
  <br />↓<br />
  <img src="https://img.shields.io/badge/2-Process%20Reservation-6DB33F?style=for-the-badge" alt="Process reservation" />
  <br />↓<br />
  <img src="https://img.shields.io/badge/3-Google%20Calendar%20Sync-4285F4?style=for-the-badge&logo=googlecalendar&logoColor=white" alt="Google Calendar synchronization" />
</p>

### Example request

```http
POST http://localhost:8080/users/13/reserve-book?bookName=DotinProject
```

### Screenshot

<p align="center">
  <img src="https://github.com/user-attachments/assets/d690004f-5068-4214-ba96-be4bdee5a81a" width="650" alt="Library application Google Calendar integration" />
</p>

### Dependency

The project references the Google Calendar API client library:

[Google Calendar API — Maven Repository](https://mvnrepository.com/artifact/com.google.apis/google-api-services-calendar/v3-rev20220715-1.32.1)

If the dependency is not already configured in `pom.xml`, add the appropriate Maven dependency rather than downloading and managing a JAR manually. Follow the project's existing dependency and authentication configuration.

---

## Terminal-Based Client

In addition to the REST API, the project includes a terminal-based client implemented in `httpClient.java`.

After login and authorization, users are presented with menus based on their roles.

| Role | General purpose |
|---|---|
| User | Browse available books and manage reservation requests |
| Admin | Manage books and perform administrative operations |
| Manager | Access manager-specific actions and permissions |

The available actions depend on the role and the corresponding permission checks in the implementation.

### User operations

The user module includes methods for:

- `showAvailableBooks()` — Display books available for reservation.
- `reservationRequest(Scanner in)` — Submit a reservation request.
- `pendingReserveBooks()` — View pending reservation requests.
- `deleteReserveRequest(Scanner in)` — Cancel a pending request.
- `showReservedBooks()` — Display approved reservations.
- `getBookNameFromUser(Scanner in)` — Handle book-name input.
- `showFilteredBooks(String stat)` — Filter reservation records.
- `showMenu(Scanner in)` — Display the user menu.

### Admin operations

The `Admin` class extends `User` and adds administrative functionality:

- `addBook()` — Add a book.
- `removeBook()` — Remove a book by ID.
- `showMenu(Scanner in)` — Display the administration menu.
- `runFuncForCommand(int choice, Scanner in)` — Execute the selected administrative action.

---

## Authentication and Security

The terminal-based implementation includes login, password verification, and role-based authorization.

### Password hashing

The implementation uses a salted SHA-256 hashing approach:

1. The user submits a username and password.
2. The system retrieves the stored salt and password hash.
3. The submitted password is hashed using the stored salt.
4. The calculated hash is compared with the stored hash.
5. Access is granted or denied based on the comparison and authorization checks.

Passwords are intended to be stored as hashes rather than plaintext.

**Security note:** Salted SHA-256 is useful for demonstrating hashing concepts, but SHA-256 is not a password-hashing algorithm designed to resist brute-force attacks. For production systems, use a dedicated password-hashing algorithm such as Argon2id, bcrypt, or PBKDF2 with an appropriate work factor. Also verify that sensitive credentials and configuration files are excluded from version control.

---

## Getting Started

### Prerequisites

- Java Development Kit (JDK) compatible with the project configuration
- Maven, if the project uses Maven
- An IDE such as IntelliJ IDEA or Eclipse (optional)
- MongoDB, if the configured application profile requires it
- Google Calendar API credentials, if calendar synchronization is enabled

### 1. Clone the repository

```bash
git clone https://github.com/Parinaz11/Library_Management_Dotin.git
cd Library_Management_Dotin
```

### 2. Configure dependencies and services

Open the project and check `pom.xml` for the required dependencies. Configure database connection settings and any required Google Calendar credentials according to the project's configuration.

### 3. Run the Spring Boot application

Locate `LibraryApplication.java` and run its `main` method from your IDE.

If the project has a Maven wrapper, you can also start it from the terminal:

```bash
./mvnw spring-boot:run
```

On Windows:

```powershell
.\mvnw.cmd spring-boot:run
```

If no Maven wrapper is included, use Maven directly:

```bash
mvn spring-boot:run
```

### 4. Explore the application

Once the application starts, open:

- **Swagger UI:** `http://localhost:8080/swagger-ui/index.html`
- **H2 Console:** `http://localhost:8080/h2-console`

You can also test the REST endpoints using Postman.

---

## Technologies Used

<p align="center">
  <img src="https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white" alt="Java" />
  <img src="https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white" alt="Spring Boot" />
  <img src="https://img.shields.io/badge/Hibernate-59666C?style=flat-square&logo=hibernate&logoColor=white" alt="Hibernate" />
  <img src="https://img.shields.io/badge/H2%20Database-1F6E8C?style=flat-square" alt="H2 Database" />
  <img src="https://img.shields.io/badge/MongoDB-47A248?style=flat-square&logo=mongodb&logoColor=white" alt="MongoDB" />
  <img src="https://img.shields.io/badge/Swagger-85EA2D?style=flat-square&logo=swagger&logoColor=black" alt="Swagger" />
  <img src="https://img.shields.io/badge/Google%20Calendar-4285F4?style=flat-square&logo=googlecalendar&logoColor=white" alt="Google Calendar" />
  <img src="https://img.shields.io/badge/Postman-FF6C37?style=flat-square&logo=postman&logoColor=white" alt="Postman" />
</p>

---

## Learning Outcomes

This project provides practical experience with:

- Designing and implementing RESTful APIs using Spring Boot.
- Structuring an application using a layered architecture and dependency injection.
- Working with JPA, Entity Manager, and database persistence.
- Implementing authentication, authorization, and role-based menus.
- Handling validation and application errors with custom exceptions.
- Adding cross-cutting logging with SLF4J and AOP.
- Integrating an external service through the Google Calendar API.
- Testing endpoints and documenting APIs using Swagger and Postman.
- Comparing a terminal-based client and a REST-based application interface.

---

<p align="center">
  <b>Java · Spring Boot · REST APIs · Database Management · External Integrations</b>
</p>
