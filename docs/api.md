# API Documentation

## Overview

This section defines the REST API structure for **EduCore**.

Each endpoint should clearly define:

* HTTP method
* URL
* purpose
* authorization
* path params
* query params
* request body
* request DTO
* response body
* status codes

---

## API Standards

### Base URL

```txt
/api
```

### Authorization Header

```http
Authorization: Bearer <access_token>
```

### Content Types

For normal requests:

```http
Content-Type: application/json
```

For file upload:

```http
Content-Type: multipart/form-data
```

---

## Common Response Models

### Generic API Response DTO

```java
public class ApiResponseDto<T> {
    private boolean success;
    private String message;
    private T data;
    private Object errors;
    private LocalDateTime timestamp;
}
```

### Pagination Response DTO

```java
public class PageResponseDto<T> {
    private List<T> content;
    private int page;
    private int size;
    private long totalElements;
    private int totalPages;
}
```

### Error Response DTO

```java
public class ErrorResponseDto {
    private boolean success;
    private String message;
    private Object errors;
    private LocalDateTime timestamp;
}
```

---

## 1. Auth API

## POST `/api/auth/register`

Register a new academy manager.

### Authorization

* Public

### Request DTO

```java
public class RegisterRequestDto {
    private String academyName;
    private String firstName;
    private String lastName;
    private String email;
    private String password;
    private String phone;
}
```

### Request Body

```json
{
  "academyName": "EduCore Academy",
  "firstName": "Elshan",
  "lastName": "Hasanov",
  "email": "owner@educore.com",
  "password": "StrongPassword123",
  "phone": "+994501234567"
}
```

### Responses

#### `201 Created`

```json
{
  "success": true,
  "message": "Account registered successfully",
  "data": {
    "userId": 1,
    "academyId": 1,
    "email": "owner@educore.com"
  }
}
```

#### `400 Bad Request`

```json
{
  "success": false,
  "message": "Validation failed"
}
```

---

## POST `/api/auth/login`

Authenticate user and return tokens.

### Authorization

* Public

### Request DTO

```java
public class LoginRequestDto {
    private String email;
    private String password;
}
```

### Request Body

```json
{
  "email": "owner@educore.com",
  "password": "StrongPassword123"
}
```

### Response DTO

```java
public class LoginResponseDto {
    private String accessToken;
    private String refreshToken;
    private String tokenType;
    private Long userId;
    private String role;
}
```

### Responses

#### `200 OK`

```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "accessToken": "jwt-access-token",
    "refreshToken": "jwt-refresh-token",
    "tokenType": "Bearer",
    "userId": 1,
    "role": "ACADEMY_MANAGER"
  }
}
```

#### `401 Unauthorized`

```json
{
  "success": false,
  "message": "Invalid email or password"
}
```

---

## POST `/api/auth/refresh`

Refresh access token.

### Authorization

* Refresh token flow

### Request Body

```json
{
  "refreshToken": "jwt-refresh-token"
}
```

### Response

```json
{
  "success": true,
  "message": "Token refreshed",
  "data": {
    "accessToken": "new-access-token"
  }
}
```

---

## POST `/api/auth/logout`

Logout current user.

### Authorization

* Authenticated user

### Response

```json
{
  "success": true,
  "message": "Logged out successfully"
}
```

---

## 2. Academy API

## GET `/api/academies/me`

Get current academy data.

### Authorization

* `ACADEMY_MANAGER`

### Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "EduCore Academy",
    "logoUrl": "/uploads/logo.png",
    "email": "info@educore.com",
    "phone": "+994501234567",
    "address": "Baku, Azerbaijan"
  }
}
```

---

## PUT `/api/academies/me`

Update academy profile.

### Authorization

* `ACADEMY_MANAGER`

### Request Body

```json
{
  "name": "EduCore Academy",
  "description": "Modern academy management solution",
  "email": "info@educore.com",
  "phone": "+994501234567",
  "address": "Baku, Azerbaijan",
  "website": "https://educore.com"
}
```

---

## 3. Student API

## POST `/api/students`

Create student.

### Authorization

* `ACADEMY_MANAGER`, `STAFF`

### Request DTO

```java
public class StudentCreateRequestDto {
    private String firstName;
    private String lastName;
    private String email;
    private String phone;
    private LocalDate birthDate;
    private String parentName;
    private String parentPhone;
}
```

### Request Body

```json
{
  "firstName": "Ali",
  "lastName": "Mammadov",
  "email": "ali@example.com",
  "phone": "+994501111111",
  "birthDate": "2010-05-10",
  "parentName": "Rashad Mammadov",
  "parentPhone": "+994502222222"
}
```

### Response

```json
{
  "success": true,
  "message": "Student created successfully",
  "data": {
    "id": 5,
    "firstName": "Ali",
    "lastName": "Mammadov",
    "email": "ali@example.com",
    "status": "ACTIVE"
  }
}
```

---

## GET `/api/students`

Get paginated students list.

### Authorization

* `ACADEMY_MANAGER`, `STAFF`

### Query Params

* `page`
* `size`
* `search`
* `status`
* `groupId`

### Example

```http
GET /api/students?page=0&size=10&search=Ali
```

### Response

```json
{
  "success": true,
  "data": {
    "content": [
      {
        "id": 5,
        "firstName": "Ali",
        "lastName": "Mammadov",
        "email": "ali@example.com",
        "phone": "+994501111111",
        "status": "ACTIVE"
      }
    ],
    "page": 0,
    "size": 10,
    "totalElements": 1,
    "totalPages": 1
  }
}
```

---

## GET `/api/students/{id}`

Get student details.

### Authorization

* `ACADEMY_MANAGER`, `STAFF`, `TEACHER`, `PARENT`, `STUDENT`

### Path Params

* `id`

### Response

```json
{
  "success": true,
  "data": {
    "id": 5,
    "firstName": "Ali",
    "lastName": "Mammadov",
    "email": "ali@example.com",
    "phone": "+994501111111",
    "birthDate": "2010-05-10",
    "parentName": "Rashad Mammadov",
    "parentPhone": "+994502222222",
    "status": "ACTIVE"
  }
}
```

---

## PUT `/api/students/{id}`

Update student.

### Authorization

* `ACADEMY_MANAGER`, `STAFF`

### Path Params

* `id`

### Request Body

```json
{
  "firstName": "Ali",
  "lastName": "Mammadov",
  "phone": "+994503333333",
  "parentPhone": "+994504444444"
}
```

---

## DELETE `/api/students/{id}`

Delete or deactivate student.

### Authorization

* `ACADEMY_MANAGER`

### Path Params

* `id`

### Response

```json
{
  "success": true,
  "message": "Student deleted successfully"
}
```

---

## 4. Teacher API

## POST `/api/teachers`

Create teacher.

### Authorization

* `ACADEMY_MANAGER`

### Request Body

```json
{
  "firstName": "Leyla",
  "lastName": "Aliyeva",
  "email": "leyla@educore.com",
  "phone": "+994505555555",
  "specialization": "Java",
  "salary": 1200
}
```

### Response

```json
{
  "success": true,
  "message": "Teacher created successfully",
  "data": {
    "id": 3,
    "firstName": "Leyla",
    "lastName": "Aliyeva"
  }
}
```

---

## GET `/api/teachers`

List teachers.

### Authorization

* `ACADEMY_MANAGER`, `STAFF`

### Query Params

* `page`
* `size`
* `search`
* `specialization`

---

## GET `/api/teachers/{id}`

Get teacher details.

### Authorization

* `ACADEMY_MANAGER`, `STAFF`, `TEACHER`

---

## PUT `/api/teachers/{id}`

Update teacher.

### Authorization

* `ACADEMY_MANAGER`

---

## DELETE `/api/teachers/{id}`

Delete or deactivate teacher.

### Authorization

* `ACADEMY_MANAGER`

---

## 5. Course API

## POST `/api/courses`

Create course.

### Authorization

* `ACADEMY_MANAGER`

### Request Body

```json
{
  "title": "Java Beginner",
  "description": "Introduction to Java",
  "category": "Programming",
  "level": "BEGINNER",
  "durationInWeeks": 12,
  "price": 300
}
```

### Response

```json
{
  "success": true,
  "message": "Course created successfully",
  "data": {
    "id": 1,
    "title": "Java Beginner",
    "level": "BEGINNER",
    "price": 300
  }
}
```

---

## GET `/api/courses`

Get courses list.

### Authorization

* Authenticated or public depending on setup

### Query Params

* `page`
* `size`
* `search`
* `level`
* `category`

---

## GET `/api/courses/{id}`

Get course details.

### Authorization

* Authenticated or public depending on setup

---

## PUT `/api/courses/{id}`

Update course.

### Authorization

* `ACADEMY_MANAGER`

---

## DELETE `/api/courses/{id}`

Delete course.

### Authorization

* `ACADEMY_MANAGER`

---

## 6. Group API

## POST `/api/groups`

Create group.

### Authorization

* `ACADEMY_MANAGER`

### Request Body

```json
{
  "name": "Java Beginner - Group A",
  "courseId": 1,
  "teacherId": 3,
  "capacity": 20,
  "startDate": "2026-05-01",
  "endDate": "2026-07-31",
  "classroom": "Room 201",
  "meetingLink": null
}
```

### Response

```json
{
  "success": true,
  "message": "Group created successfully",
  "data": {
    "id": 1,
    "name": "Java Beginner - Group A",
    "courseId": 1,
    "teacherId": 3
  }
}
```

---

## POST `/api/groups/{id}/students/{studentId}`

Assign student to group.

### Authorization

* `ACADEMY_MANAGER`, `STAFF`

### Path Params

* `id`
* `studentId`

### Response

```json
{
  "success": true,
  "message": "Student assigned to group successfully"
}
```

---

## GET `/api/groups`

List groups.

### Query Params

* `page`
* `size`
* `courseId`
* `teacherId`
* `status`

---

## 7. Schedule API

## POST `/api/schedules`

Create schedule entry.

### Authorization

* `ACADEMY_MANAGER`

### Request Body

```json
{
  "groupId": 1,
  "teacherId": 3,
  "weekday": "MONDAY",
  "startTime": "10:00",
  "endTime": "12:00",
  "room": "Room 201",
  "meetingLink": null
}
```

### Business Rules

* teacher cannot be double-booked
* room cannot be double-booked
* time must be valid
* group must belong to current academy

### Response

```json
{
  "success": true,
  "message": "Schedule created successfully"
}
```

---

## GET `/api/schedules`

Get schedules.

### Query Params

* `groupId`
* `teacherId`
* `weekday`

---

## 8. Attendance API

## POST `/api/attendance`

Mark attendance.

### Authorization

* `TEACHER`, `ACADEMY_MANAGER`

### Request DTO

```java
public class AttendanceMarkRequestDto {
    private Long studentId;
    private Long groupId;
    private LocalDate date;
    private String status;
    private String note;
}
```

### Request Body

```json
{
  "studentId": 5,
  "groupId": 1,
  "date": "2026-05-03",
  "status": "PRESENT",
  "note": "On time"
}
```

### Response

```json
{
  "success": true,
  "message": "Attendance marked successfully"
}
```

---

## GET `/api/attendance/student/{studentId}`

Get attendance history.

### Authorization

* `ACADEMY_MANAGER`, `TEACHER`, `PARENT`, `STUDENT`

### Query Params

* `from`
* `to`
* `groupId`

---

## 9. Payment API

## POST `/api/payments`

Create payment record.

### Authorization

* `ACADEMY_MANAGER`, `STAFF`

### Request Body

```json
{
  "studentId": 5,
  "amount": 300,
  "paidAmount": 150,
  "dueDate": "2026-05-10",
  "paymentMethod": "CASH",
  "status": "PARTIAL",
  "note": "First installment"
}
```

### Response

```json
{
  "success": true,
  "message": "Payment saved successfully"
}
```

---

## GET `/api/payments`

Get payments list.

### Query Params

* `studentId`
* `status`
* `from`
* `to`
* `page`
* `size`

---

## GET `/api/payments/student/{studentId}`

Get student payment history.

---

## 10. Assignment API

## POST `/api/assignments`

Create assignment.

### Authorization

* `TEACHER`

### Request Body

```json
{
  "groupId": 1,
  "title": "Java Variables Homework",
  "description": "Complete exercises 1 to 10",
  "deadline": "2026-05-20T23:59:00",
  "attachmentUrl": "/uploads/assignment1.pdf"
}
```

---

## POST `/api/assignments/{id}/submit`

Submit assignment.

### Authorization

* `STUDENT`

### Content-Type

* `multipart/form-data`

### Path Params

* `id`

### Form Data

* `file`

### Response

```json
{
  "success": true,
  "message": "Assignment submitted successfully"
}
```

---

## GET `/api/assignments/group/{groupId}`

List assignments for group.

---

## 11. Exam API

## POST `/api/exams`

Create exam.

### Authorization

* `TEACHER`, `ACADEMY_MANAGER`

### Request Body

```json
{
  "groupId": 1,
  "title": "Java Midterm",
  "description": "Midterm exam",
  "examDate": "2026-06-15",
  "totalScore": 100
}
```

---

## POST `/api/exams/{id}/grades`

Enter grades.

### Authorization

* `TEACHER`

### Request Body

```json
{
  "grades": [
    {
      "studentId": 5,
      "score": 90
    },
    {
      "studentId": 6,
      "score": 75
    }
  ]
}
```

### Response

```json
{
  "success": true,
  "message": "Grades saved successfully"
}
```

---

## 12. Materials API

## POST `/api/materials`

Create link-based material.

### Authorization

* `TEACHER`, `ACADEMY_MANAGER`

### Request Body

```json
{
  "courseId": 1,
  "title": "Lesson 1 Video",
  "type": "VIDEO_LINK",
  "videoUrl": "https://youtube.com/example",
  "description": "Introduction lesson"
}
```

---

## POST `/api/materials/upload`

Upload file material.

### Authorization

* `TEACHER`, `ACADEMY_MANAGER`

### Content-Type

* `multipart/form-data`

### Form Data

* `courseId`
* `title`
* `type`
* `file`
* `description`

---

## GET `/api/materials/course/{courseId}`

List materials by course.

---

## 13. Notification API

## GET `/api/notifications/me`

Get current user notifications.

### Authorization

* Authenticated users

### Response

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "title": "Payment Reminder",
      "message": "Your payment is due tomorrow",
      "isRead": false,
      "createdAt": "2026-05-01T11:00:00"
    }
  ]
}
```

---

## PUT `/api/notifications/{id}/read`

Mark notification as read.

### Authorization

* Authenticated users

---

## 14. Dashboard API

## GET `/api/dashboard/manager`

Get manager dashboard statistics.

### Authorization

* `ACADEMY_MANAGER`

### Response

```json
{
  "success": true,
  "data": {
    "totalStudents": 120,
    "totalTeachers": 12,
    "activeCourses": 15,
    "monthlyRevenue": 5400,
    "unpaidStudents": 7,
    "todayClasses": 9
  }
}
```

---

## GET `/api/dashboard/teacher`

Get teacher dashboard data.

### Authorization

* `TEACHER`

---

## GET `/api/dashboard/student`

Get student dashboard data.

### Authorization

* `STUDENT`

---

## 15. Reports API

## GET `/api/reports/attendance`

Get attendance report.

### Authorization

* `ACADEMY_MANAGER`

### Query Params

* `groupId`
* `studentId`
* `from`
* `to`

---

## GET `/api/reports/payments`

Get payments report.

### Authorization

* `ACADEMY_MANAGER`

### Query Params

* `status`
* `from`
* `to`

---

## GET `/api/reports/performance`

Get student performance report.

### Authorization

* `ACADEMY_MANAGER`, `TEACHER`

---

## Example Development Order for API

### Phase 1

* auth
* academy
* students
* teachers

### Phase 2

* courses
* groups
* schedules
* attendance

### Phase 3

* payments
* assignments
* exams
* materials

### Phase 4

* notifications
* dashboard
* reports

If you want, I can now rewrite this into **three separate copy-paste-ready GitHub files**:
`FRONTEND.md`, `BACKEND.md`, and `API.md`.
