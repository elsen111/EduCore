# Frontend Documentation

## Overview

The frontend of **EduCore** is a modern web application built with **React** and **TypeScript**.

Its responsibilities are:

* rendering public and dashboard interfaces
* managing navigation and protected routes
* handling forms and client-side validation
* connecting to backend APIs
* displaying role-based content
* supporting responsive design
* managing UI state and authentication state

---

## Frontend Tech Stack

* **React**
* **TypeScript**
* **Vite**
* **Tailwind CSS**
* **React Router**
* **Axios**
* **React Hook Form**
* **Zod**
* **i18n**
* **Recharts** or **Chart.js**
* **Lucide React**

---

## Frontend Responsibilities

The frontend is responsible for:

* public marketing pages
* authentication pages
* academy dashboard
* student, teacher, course, and group management pages
* schedule and attendance pages
* payment, assignment, exam, and materials pages
* role-based UI rendering
* API request and response handling
* token storage and route protection
* reusable component system

---

## Frontend Architecture

The frontend should follow a modular and scalable structure.

### Main architectural ideas

* reusable components
* feature-based page separation
* service-based API communication
* typed request and response handling
* context-based auth and app settings
* protected dashboard routes
* validation schemas separated from UI

---

## Frontend Pages

## Public Pages

### Home Page

Purpose:

* introduce the platform
* explain benefits
* display featured courses
* provide call-to-action buttons

### About Page

Purpose:

* show academy or platform information
* mission and vision
* short team or business intro

### Courses Page

Purpose:

* display all available courses
* search and filter courses

### Course Details Page

Purpose:

* show single course details
* syllabus
* duration
* price
* teacher
* enroll information

### Teachers Page

Purpose:

* show teacher cards and profiles

### Contact Page

Purpose:

* contact form
* address
* phone
* email
* map section

### Login Page

Purpose:

* authenticate users
* redirect user based on role

### Register Page

Purpose:

* academy owner registration

---

## Dashboard Pages

### Dashboard Home

Purpose:

* role-based statistics
* overview cards
* charts
* quick actions

### Students Page

Purpose:

* list all students
* search, filter, paginate
* add, edit, delete

### Student Details Page

Purpose:

* student profile
* attendance history
* payment history
* enrolled groups

### Teachers Page

Purpose:

* list teachers
* teacher CRUD operations

### Teacher Details Page

Purpose:

* show teacher information
* assigned groups
* working schedule

### Courses Page

Purpose:

* manage courses

### Course Create / Edit Page

Purpose:

* create new course
* update existing course

### Groups Page

Purpose:

* manage class groups
* assign teacher and students

### Schedule Page

Purpose:

* weekly and daily schedule management

### Attendance Page

Purpose:

* mark attendance
* filter by group and date

### Payments Page

Purpose:

* fee tracking
* payment history
* debt overview

### Assignments Page

Purpose:

* create and manage homework
* view submissions

### Exams Page

Purpose:

* create exams
* enter and review grades

### Materials Page

Purpose:

* upload documents
* manage learning materials
* add video links

### Notifications Page

Purpose:

* display system notifications and reminders

### Reports Page

Purpose:

* show analytics and reports
* prepare exports

### Settings Page

Purpose:

* academy profile settings
* branding
* password changes
* general preferences

---

## Frontend Components

## Shared UI Components

* `Button`
* `Input`
* `Textarea`
* `Select`
* `Checkbox`
* `Radio`
* `Table`
* `Pagination`
* `Card`
* `Badge`
* `Tabs`
* `Modal`
* `Drawer`
* `Alert`
* `Loader`
* `EmptyState`
* `ConfirmDialog`

## Layout Components

* `PublicLayout`
* `DashboardLayout`
* `Sidebar`
* `Topbar`
* `Footer`
* `ProtectedRoute`

## Feature Components

* `StudentForm`
* `TeacherForm`
* `CourseForm`
* `GroupForm`
* `ScheduleCalendar`
* `AttendanceTable`
* `PaymentTable`
* `AssignmentForm`
* `ExamForm`
* `MaterialUploadForm`
* `DashboardStatsCards`

---

## Frontend State Management

Recommended approach:

* **React Context** for auth and app-wide settings
* local state for page-specific UI logic
* optional **Zustand** or **Redux Toolkit** if project grows larger

### Suggested contexts

* `AuthContext`
* `UserContext`
* `LanguageContext`
* `ThemeContext`
* `NotificationContext`

---

## Frontend Routing

Example route structure:

```txt
/
├── /
├── /about
├── /courses
├── /courses/:id
├── /teachers
├── /contact
├── /login
├── /register
├── /dashboard
│   ├── /dashboard/students
│   ├── /dashboard/students/:id
│   ├── /dashboard/teachers
│   ├── /dashboard/teachers/:id
│   ├── /dashboard/courses
│   ├── /dashboard/courses/create
│   ├── /dashboard/courses/:id/edit
│   ├── /dashboard/groups
│   ├── /dashboard/schedule
│   ├── /dashboard/attendance
│   ├── /dashboard/payments
│   ├── /dashboard/assignments
│   ├── /dashboard/exams
│   ├── /dashboard/materials
│   ├── /dashboard/notifications
│   ├── /dashboard/reports
│   └── /dashboard/settings
```

---

## Frontend Form Handling

Recommended libraries:

* **React Hook Form**
* **Zod**

Used for:

* login form
* register form
* student create/edit form
* teacher create/edit form
* course create/edit form
* group create/edit form
* payment form
* assignment form
* exam form

---

## Frontend Validation

Client-side validation should cover:

* required fields
* email format
* password strength
* min/max lengths
* valid numbers
* valid dates
* file type validation
* file size validation
* simple schedule conflict prevention before submission if needed

---

## Frontend API Integration

All API calls should be separated into service files.

### Suggested services

* `authService.ts`
* `academyService.ts`
* `studentService.ts`
* `teacherService.ts`
* `courseService.ts`
* `groupService.ts`
* `scheduleService.ts`
* `attendanceService.ts`
* `paymentService.ts`
* `assignmentService.ts`
* `examService.ts`
* `materialService.ts`
* `notificationService.ts`
* `dashboardService.ts`
* `reportService.ts`

### Example Axios setup

```ts
import axios from "axios";

export const api = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL,
});

api.interceptors.request.use((config) => {
  const token = localStorage.getItem("accessToken");

  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }

  return config;
});
```

---

## Frontend Role-Based Access

The frontend must show and hide pages depending on the current user role.

### Example roles

* `SUPER_ADMIN`
* `ACADEMY_MANAGER`
* `TEACHER`
* `STUDENT`
* `PARENT`
* `STAFF`

### Example access behavior

* Academy manager can manage students, teachers, courses, groups, payments
* Teacher can access assigned groups, attendance, assignments, grades
* Student can access own courses, assignments, grades, payments
* Parent can access child attendance, grades, and payment status

### Example protected route

```tsx
type ProtectedRouteProps = {
  roles?: string[];
  children: React.ReactNode;
};

function ProtectedRoute({ roles, children }: ProtectedRouteProps) {
  const user = useAuth();

  if (!user) return <Navigate to="/login" />;
  if (roles && !roles.includes(user.role)) return <Navigate to="/dashboard" />;

  return <>{children}</>;
}
```

---

## Suggested Frontend Folder Structure

```bash
frontend/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   │   ├── common/
│   │   ├── layout/
│   │   └── feature/
│   ├── pages/
│   │   ├── public/
│   │   ├── auth/
│   │   └── dashboard/
│   ├── routes/
│   ├── services/
│   ├── hooks/
│   ├── context/
│   ├── schemas/
│   ├── types/
│   ├── utils/
│   ├── constants/
│   ├── i18n/
│   ├── App.tsx
│   └── main.tsx
```

---

