# EduCore — Academy Management and Learning Platform

A full-stack academy management SaaS platform built for training centers, language schools, tutoring centers, and educational institutes.

EduCore helps academies manage students, teachers, schedules, attendance, payments, assignments, exams, and course materials in one centralized system.

The project combines a modern frontend built with React and a powerful backend built with Java Spring Boot.

---

## 🚀 Project Purpose

Many academies still manage operations manually using spreadsheets, paper notes, and messaging apps.

EduCore aims to digitize the full workflow of an academy by providing:

* Student management
* Teacher management
* Course management
* Attendance tracking
* Payment tracking
* Scheduling
* Homework and exams
* Notifications
* Reports and analytics
* Public website pages

---

## 🎯 Main Goals

* Centralize all academy operations in one platform
* Reduce manual work and mistakes
* Improve communication between teachers, students, parents, and staff
* Provide detailed reports and analytics
* Support multiple user roles
* Create a scalable SaaS architecture for future business growth

---

## 👥 User Roles

### 1. Super Admin

The system owner who manages the whole platform.

#### Permissions

* Manage all academies
* View platform-wide statistics
* Activate or deactivate academy accounts
* Manage subscription plans
* Control system settings
* Monitor all users

### 2. Academy Owner / Manager

The academy owner who uses the platform.

#### Permissions

* Manage academy profile
* Add teachers and students
* Create courses and groups
* Manage schedules
* Track payments
* View reports
* Manage staff accounts

### 3. Teacher

#### Permissions

* View assigned courses and groups
* Mark attendance
* Upload materials
* Create homework
* Enter exam results
* Communicate with students

### 4. Student

#### Permissions

* View enrolled courses
* Access schedules
* Submit homework
* View grades
* Watch course materials
* Check payment status

### 5. Parent

#### Permissions

* Monitor child attendance
* View grades and performance
* Check payment status
* Receive notifications

### 6. Receptionist / Staff

#### Permissions

* Register students
* Manage student requests
* Update schedules
* Track fee collection
* Handle administrative tasks

---

## 🧩 Main Modules

### Authentication Module

#### Features

* Register academy account
* Login and logout
* Forgot password
* Reset password
* JWT authentication
* Refresh tokens
* Role-based access control
* Protected routes

---

### Academy Profile Module

#### Features

* Academy name
* Logo upload
* Description
* Contact information
* Address
* Social media links
* Working hours
* Branch management
* Subscription plan details

---

### Student Management Module

#### Features

* Add, edit, delete students
* Student profile page
* Student photo
* Parent information
* Emergency contact
* Enrollment date
* Assigned courses
* Assigned groups
* Attendance history
* Payment history
* Notes and comments
* Student search and filtering

---

### Teacher Management Module

#### Features

* Add, edit, delete teachers
* Teacher profile
* Assigned subjects
* Assigned groups
* Salary information
* Available hours
* Performance notes
* Teacher attendance tracking

---

### Course Management Module

#### Features

* Create and manage courses
* Course title and description
* Course level
* Duration
* Price
* Category
* Thumbnail image
* Syllabus
* Teacher assignment
* Lesson materials
* Video lessons

#### Example Courses

* Java Beginner
* English A1
* IELTS Preparation
* React Frontend Bootcamp

---

### Group / Batch Module

#### Features

* Group name
* Course assignment
* Teacher assignment
* Student list
* Start and end dates
* Maximum capacity
* Classroom or online link
* Weekly schedule

#### Example Groups

* Java Beginner — Group A
* Java Beginner — Group B
* English A1 — Morning Group

---

### Schedule Module

#### Features

* Weekly timetable
* Daily timetable
* Assign teachers and rooms
* Online meeting links
* Conflict detection
* Prevent double-booking of teachers
* Prevent classroom overlaps
* Calendar view

---

### Attendance Module

#### Features

* Mark present / absent / late
* Attendance history
* Attendance percentage
* Date-based filtering
* Parent notifications for absences

---

### Payment Module

#### Features

* Monthly fee tracking
* Payment records
* Discounts
* Remaining debt
* Invoice generation
* Payment reminders
* Payment history

---

### Homework / Assignment Module

#### Features

* Create homework
* Attach files or links
* Set deadlines
* Student submission
* Teacher review
* Feedback system

---

### Exam / Grade Module

#### Features

* Create exams
* Enter scores
* Grade history
* Pass/fail logic
* Student performance tracking
* Downloadable result sheets

---

### Lesson Materials Module

#### Features

* Upload PDFs
* Upload images
* Upload documents
* Add external links
* Embed YouTube videos
* Organize materials by lesson

---

### Notification Module

#### Features

* In-app notifications
* Homework reminders
* Payment reminders
* Schedule updates
* Class cancellation alerts
* Exam announcements

---

### Dashboard & Analytics Module

#### Academy Dashboard

* Total students
* Total teachers
* Active courses
* Unpaid students
* Monthly revenue
* Attendance statistics
* Today's classes

#### Teacher Dashboard

* Today's schedule
* Homework reviews
* Attendance tasks

#### Student Dashboard

* Enrolled courses
* Upcoming lessons
* Assignments due
* Grades
* Payment status

---

### Reports Module

#### Features

* Attendance reports
* Payment reports
* Student performance reports
* Teacher workload reports
* Course popularity reports
* Export to PDF
* Export to Excel

---

### Public Website Module

#### Public Pages

* Home page
* About page
* Courses page
* Course details page
* Teachers page
* Contact page
* Registration page
* Login page

---

## 🏗️ Recommended MVP

The first version should focus only on the core features.

### Must-Have Features

* Authentication
* Role system
* Student CRUD
* Teacher CRUD
* Course CRUD
* Group CRUD
* Schedule management
* Attendance tracking
* Payment tracking
* Dashboard statistics
* File upload for materials

### Nice-to-Have Features

* Parent panel
* Homework submission
* Exam module
* Notifications
* Certificates
* Subscription plans
* Video lessons

---

## 🛠️ Tech Stack

### Frontend

* React
* TypeScript
* Tailwind CSS
* React Router
* Axios
* React Hook Form
* Zod
* i18n
* Chart.js or Recharts

### Backend

* Java
* Spring Boot
* Spring Security
* JWT Authentication
* JPA / Hibernate
* PostgreSQL
* Swagger / OpenAPI
* Bean Validation
* File Upload Handling

### Deployment

* Docker
* Nginx
* VPS Hosting
* PostgreSQL Server

---

## 🗄️ Database Entities

Main database tables:

* users
* roles
* academies
* branches
* students
* parents
* teachers
* courses
* groups
* enrollments
* schedules
* attendance
* payments
* assignments
* submissions
* exams
* grades
* materials
* notifications

---

## 📁 Suggested Project Structure

```bash
frontend/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   ├── layouts/
│   ├── pages/
│   ├── routes/
│   ├── services/
│   ├── hooks/
│   ├── context/
│   ├── types/
│   ├── utils/
│   └── i18n/

backend/
├── src/main/java/
│   ├── controller/
│   ├── service/
│   ├── repository/
│   ├── entity/
│   ├── dto/
│   ├── mapper/
│   ├── security/
│   ├── config/
│   └── exception/
```

---

## 📹 Video Upload Strategy

For the graduation version, the easiest approach is:

* Upload PDFs and images directly
* Store lesson videos as embedded YouTube unlisted links
* Save the video URL in the database
* Optionally use free cloud storage like Cloudinary later

This avoids:

* Large server storage costs
* Slow streaming
* Complex video processing
* Expensive hosting

---

## 💡 Business Value

EduCore can become a real SaaS platform for:

* Language schools
* Coding bootcamps
* Private tutors
* Music schools
* Art academies
* Corporate training centers

Potential business model:

* Monthly subscription per academy
* Free trial plan
* Premium features for larger academies
* White-label academy branding

---

## 🎓 Graduation Project Presentation

### Problem

Many academies still use manual methods to manage students, schedules, attendance, and payments.

### Solution

EduCore provides a centralized digital platform that simplifies academy management.

### Benefits

* Saves time
* Reduces errors
* Improves organization
* Makes communication easier
* Supports academy growth
* Provides analytics and reports

---

## ⚙️ Installation and Setup

### 1. Clone the Repository

```bash id="b9tdfu"
git clone https://github.com/your-username/educore.git
cd educore
```

---

## 🖥️ Frontend Setup

### 2. Navigate to Frontend Folder

```bash id="7xmxkm"
cd frontend
```

### 3. Install Dependencies

```bash id="lb4z8d"
npm install
```

### 4. Create Environment Variables

Create a `.env` file inside the frontend folder:

```env id="l2o9sd"
VITE_API_BASE_URL=http://localhost:8080/api
```

### 5. Run Frontend Development Server

```bash id="2a8hmy"
npm run dev
```

Frontend will run on:

```bash id="l6qmd9"
http://localhost:5173
```

---

## ☕ Backend Setup

### 6. Navigate to Backend Folder

```bash id="r7t2zc"
cd backend
```

### 7. Configure Database

Create a PostgreSQL database:

```sql id="f0p2wr"
CREATE DATABASE educore_db;
```

### 8. Configure Application Properties

Open:

```bash id="g8y4nv"
src/main/resources/application.properties
```

Example configuration:

```properties id="g3h7as"
spring.datasource.url=jdbc:postgresql://localhost:5432/educore_db
spring.datasource.username=postgres
spring.datasource.password=your_password

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

server.port=8080

jwt.secret=your_super_secret_key
jwt.expiration=86400000
```

### 9. Install Backend Dependencies

If using Maven:

```bash id="vd3uxr"
mvn clean install
```

If using Gradle:

```bash id="i5mznx"
./gradlew build
```

### 10. Run Backend Server

If using Maven:

```bash id="cl0jgs"
mvn spring-boot:run
```

If using Gradle:

```bash id="ndah9u"
./gradlew bootRun
```

Backend will run on:

```bash id="xk8qfr"
http://localhost:8080
```

---

## 🐳 Running with Docker

### 11. Build and Start Containers

```bash id="y4l2qp"
docker-compose up --build
```

Example `docker-compose.yml`:

```yaml id="k2d6ow"
version: '3.8'

services:
  postgres:
    image: postgres:15
    container_name: educore_db
    restart: always
    environment:
      POSTGRES_DB: educore_db
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    ports:
      - "5432:5432"

  backend:
    build: ./backend
    container_name: educore_backend
    ports:
      - "8080:8080"
    depends_on:
      - postgres
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://postgres:5432/educore_db
      SPRING_DATASOURCE_USERNAME: postgres
      SPRING_DATASOURCE_PASSWORD: postgres

  frontend:
    build: ./frontend
    container_name: educore_frontend
    ports:
      - "5173:5173"
    depends_on:
      - backend
```

---

## 🧪 Default Development Accounts

```txt id="aw50xq"
Super Admin
Email: admin@educore.com
Password: Admin123!

Academy Manager
Email: manager@educore.com
Password: Manager123!

Teacher
Email: teacher@educore.com
Password: Teacher123!

Student
Email: student@educore.com
Password: Student123!
```

---

## 📜 Available Scripts

### Frontend

```bash id="z6v8sh"
npm run dev
npm run build
npm run preview
npm run lint
```

### Backend (Maven)

```bash id="r8v3lg"
mvn spring-boot:run
mvn clean install
mvn test
```

### Backend (Gradle)

```bash id="k9m2pj"
./gradlew bootRun
./gradlew build
./gradlew test
```

