# Backend Documentation

## Overview

The backend of **EduCore** is a REST API built with **Java Spring Boot**.

Its responsibilities are:

* authentication and authorization
* business logic
* input validation
* database operations
* file handling
* role-based access control
* reporting and analytics
* secure communication with the frontend

---

## Backend Tech Stack

* **Java 17+**
* **Spring Boot**
* **Spring Security**
* **JWT**
* **Spring Data JPA**
* **Hibernate**
* **PostgreSQL**
* **Bean Validation**
* **Lombok**
* **MapStruct** or manual mappers
* **Swagger / OpenAPI**
* **Multipart File Upload**
* **Docker**

---

## Backend Architecture

The backend should follow a layered architecture.

### Layers

* Controller Layer
* Service Layer
* Repository Layer
* DTO Layer
* Entity Layer
* Security Layer
* Exception Handling Layer

---

## Core Backend Responsibilities

### Controller Layer

Responsible for:

* receiving requests
* validating inputs
* calling services
* returning structured responses

### Service Layer

Responsible for:

* business rules
* transactional logic
* authorization checks
* domain workflows

### Repository Layer

Responsible for:

* database communication
* queries
* persistence

### DTO Layer

Responsible for:

* request models
* response models
* safe API contracts
* separation from entities

### Security Layer

Responsible for:

* JWT validation
* route protection
* authentication
* current user extraction
* role checks

---

## Security

Authentication and authorization should use:

* JWT access tokens
* refresh tokens
* BCrypt password hashing
* Spring Security filter chain

### Security features

* login validation
* token generation
* access token verification
* refresh flow
* logout support
* route protection by role

---

## Database Design

The database should support:

* academy-based logical multi-tenancy
* role-based user ownership
* relationships between teachers, students, courses, and groups
* schedule and attendance data
* payment tracking
* assignment and exam workflows
* notifications

---

## Main Entities

## User

Fields:

* id
* firstName
* lastName
* email
* password
* phone
* role
* academy
* isActive
* createdAt

## Academy

Fields:

* id
* name
* logoUrl
* description
* email
* phone
* address
* website
* subscriptionPlan
* createdAt

## Student

Fields:

* id
* academy
* firstName
* lastName
* email
* phone
* birthDate
* parentName
* parentPhone
* enrolledAt
* status

## Teacher

Fields:

* id
* academy
* firstName
* lastName
* email
* phone
* specialization
* salary
* status

## Course

Fields:

* id
* academy
* title
* description
* category
* level
* durationInWeeks
* price
* thumbnailUrl
* status

## Group

Fields:

* id
* academy
* course
* teacher
* name
* capacity
* startDate
* endDate
* classroom
* meetingLink
* status

## Enrollment

Fields:

* id
* student
* group
* enrolledAt
* status

## Schedule

Fields:

* id
* group
* teacher
* weekday
* startTime
* endTime
* room
* meetingLink

## Attendance

Fields:

* id
* student
* group
* schedule
* date
* status
* note

## Payment

Fields:

* id
* student
* amount
* paidAmount
* dueDate
* paidAt
* paymentMethod
* status
* note

## Assignment

Fields:

* id
* group
* title
* description
* deadline
* attachmentUrl
* createdBy

## Submission

Fields:

* id
* assignment
* student
* fileUrl
* submittedAt
* feedback
* score

## Exam

Fields:

* id
* group
* title
* description
* examDate
* totalScore

## Grade

Fields:

* id
* exam
* student
* score
* passed

## Material

Fields:

* id
* course
* title
* type
* fileUrl
* videoUrl
* description

## Notification

Fields:

* id
* user
* title
* message
* isRead
* createdAt

---

## DTO Structure

DTOs should be divided into:

* request DTOs
* response DTOs
* summary DTOs
* filter DTOs
* pagination DTOs

### Examples

* `RegisterRequestDto`
* `LoginRequestDto`
* `LoginResponseDto`
* `StudentCreateRequestDto`
* `StudentUpdateRequestDto`
* `StudentResponseDto`
* `TeacherCreateRequestDto`
* `TeacherResponseDto`
* `CourseCreateRequestDto`
* `CourseResponseDto`
* `GroupCreateRequestDto`
* `AttendanceMarkRequestDto`
* `ApiResponseDto<T>`
* `PageResponseDto<T>`

---

## Validation

Backend validation should include:

* `@NotBlank`
* `@NotNull`
* `@Email`
* `@Size`
* `@Min`
* `@Max`
* `@Past`
* `@Future`
* custom business validation where needed

Examples:

* unique email
* no teacher schedule overlap
* no room double-booking
* group capacity limit
* academy ownership checks
* assignment submission deadline checks

---

## Exception Handling

A global exception handler should return consistent responses.

### Suggested exceptions

* `ResourceNotFoundException`
* `BadRequestException`
* `UnauthorizedException`
* `ForbiddenException`
* `ConflictException`
* `ValidationException`

### Example error response

```json
{
  "success": false,
  "message": "Student not found",
  "errors": null,
  "timestamp": "2026-04-19T12:10:00"
}
```

---

## Suggested Backend Package Structure

```bash
backend/
├── src/main/java/com/educore/
│   ├── config/
│   ├── controller/
│   ├── dto/
│   │   ├── auth/
│   │   ├── academy/
│   │   ├── student/
│   │   ├── teacher/
│   │   ├── course/
│   │   ├── group/
│   │   ├── attendance/
│   │   ├── payment/
│   │   ├── assignment/
│   │   ├── exam/
│   │   ├── material/
│   │   └── common/
│   ├── entity/
│   ├── enums/
│   ├── exception/
│   ├── mapper/
│   ├── repository/
│   ├── security/
│   ├── service/
│   └── EduCoreApplication.java
```

---

## Authorization Model

Roles used in the system:

* `SUPER_ADMIN`
* `ACADEMY_MANAGER`
* `TEACHER`
* `STUDENT`
* `PARENT`
* `STAFF`

### Example permissions

#### SUPER_ADMIN

* manage all academies
* manage subscription plans
* see platform-wide statistics

#### ACADEMY_MANAGER

* manage academy data
* manage students
* manage teachers
* manage courses and groups
* manage payments

#### TEACHER

* manage attendance for assigned groups
* create assignments
* enter grades

#### STUDENT

* see own courses
* submit homework
* view grades and payments

#### PARENT

* view linked child attendance
* view child grades
* view payment status

---

