# Backend Documentation

## Overview

The backend of **EstateFlow** is a REST API built with **Java Spring Boot**.

Its responsibilities are:

* authentication and authorization
* forgot-password with OTP and authenticated change-password workflows
* property listing business logic
* agency, owner, buyer, renter, and admin workflows
* search, messaging, inquiry, viewing, and recommendation workflows
* dashboard aggregation for each role
* database operations
* file handling
* billing, commissions, and promotion handling
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
* dashboard aggregation logic
* OTP lifecycle management
* role-specific workflows for admin, agency, owner, buyer, and renter journeys

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
* password hashing
* OTP verification
* session invalidation after password reset or password change

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
* forgot-password OTP generation
* OTP verification with expiry and max attempts
* password reset after verified OTP
* authenticated change-password flow
* refresh token revocation after password changes

### Password recovery and password change flow

The backend should support two different password update workflows.

#### Forgot-password with OTP

* user submits email or phone on the forgot-password endpoint
* backend generates a short-lived OTP
* OTP should be hashed before persistence
* OTP should expire after a limited time, such as 5 to 10 minutes
* resend should respect cooldown and rate limiting
* reset-password endpoint should only work after successful OTP verification
* all active refresh tokens should be revoked after a successful password reset

#### Change-password for authenticated users

* authenticated user submits current password, new password, and confirm password
* backend verifies current password before accepting the change
* backend should reject weak or reused passwords if password history is implemented
* password change should update `passwordUpdatedAt`
* backend may optionally invalidate other active sessions

---

## Database Design

The database should support:

* agency-based logical multi-tenancy
* role-based user ownership
* relationships between agencies, agents, owners, clients, and properties
* favorites and saved search data
* viewing appointments, inquiries, messages, and offers
* payment tracking, featured listings, and ads
* notifications and recommendation signals
* password recovery OTP records
* refresh token revocation and account security auditing

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
* agency
* isActive
* isEmailVerified
* passwordUpdatedAt
* lastLoginAt
* createdAt

## Agency

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
* status
* createdAt

## Property

Fields:

* id
* agency
* owner
* assignedAgent
* title
* description
* type
* listingType
* price
* city
* address
* status
* createdAt

## PropertyImage

Fields:

* id
* property
* imageUrl
* sortOrder
* isCover
* createdAt

## PropertyFeature

Fields:

* id
* property
* name
* value
* category

## Favorite

Fields:

* id
* user
* property
* createdAt

## SavedSearch

Fields:

* id
* user
* name
* filters
* isAlertEnabled
* createdAt

## Inquiry

Fields:

* id
* property
* sender
* recipient
* message
* status
* createdAt

## Conversation

Fields:

* id
* property
* buyerOrRenter
* recipient
* lastMessageAt
* status

## Message

Fields:

* id
* conversation
* sender
* content
* attachmentUrl
* createdAt

## Viewing

Fields:

* id
* property
* requester
* assignedTo
* scheduledAt
* status
* note
* createdAt

## Offer

Fields:

* id
* property
* buyer
* amount
* status
* counterAmount
* message
* createdAt

## Payment

Fields:

* id
* user
* property
* purpose
* amount
* provider
* status
* paidAt

## Promotion

Fields:

* id
* property
* planType
* startDate
* endDate
* status
* budget

## AdPlacement

Fields:

* id
* agency
* title
* imageUrl
* targetUrl
* startDate
* endDate
* status

## Notification

Fields:

* id
* user
* title
* message
* isRead
* createdAt

## PasswordResetOtp

Fields:

* id
* user
* deliveryChannel
* otpHash
* attemptCount
* expiresAt
* verifiedAt
* status
* createdAt

## RefreshToken

Fields:

* id
* user
* tokenHash
* expiresAt
* revokedAt
* createdAt

---

## DTO Structure

DTOs should be divided into:

* request DTOs
* response DTOs
* summary DTOs
* filter DTOs
* pagination DTOs
* dashboard DTOs

### Examples

* `RegisterRequestDto`
* `LoginRequestDto`
* `LoginResponseDto`
* `ForgotPasswordRequestDto`
* `VerifyOtpRequestDto`
* `ResetPasswordRequestDto`
* `ChangePasswordRequestDto`
* `UserProfileUpdateRequestDto`
* `PropertyCreateRequestDto`
* `PropertyUpdateRequestDto`
* `PropertyResponseDto`
* `SavedSearchCreateRequestDto`
* `InquiryCreateRequestDto`
* `ViewingCreateRequestDto`
* `OfferCreateRequestDto`
* `PaymentCreateRequestDto`
* `AdminDashboardResponseDto`
* `AgencyDashboardResponseDto`
* `OwnerDashboardResponseDto`
* `BuyerDashboardResponseDto`
* `RenterDashboardResponseDto`
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
* listing ownership checks
* valid sale and rent pricing
* image count and file type limits
* viewing slot conflict prevention
* offer amount rules
* promotion eligibility checks
* agency ownership checks
* OTP expiry checks
* OTP attempt limit checks
* strong password policy validation
* current password verification for change-password

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
* `OtpExpiredException`
* `OtpAttemptsExceededException`

### Example error response

```json
{
  "success": false,
  "message": "OTP has expired",
  "errors": null,
  "timestamp": "2026-04-19T12:10:00"
}
```

---

## Suggested Backend Package Structure

```bash
backend/
├── src/main/java/com/estateflow/
│   ├── config/
│   ├── controller/
│   │   ├── admin/
│   │   ├── auth/
│   │   ├── agency/
│   │   ├── dashboard/
│   │   └── user/
│   ├── dto/
│   │   ├── admin/
│   │   ├── auth/
│   │   ├── agency/
│   │   ├── dashboard/
│   │   ├── property/
│   │   ├── search/
│   │   ├── inquiry/
│   │   ├── message/
│   │   ├── viewing/
│   │   ├── offer/
│   │   ├── payment/
│   │   ├── promotion/
│   │   ├── notification/
│   │   └── common/
│   ├── entity/
│   ├── enums/
│   ├── exception/
│   ├── mapper/
│   ├── repository/
│   ├── security/
│   ├── service/
│   │   ├── auth/
│   │   ├── dashboard/
│   │   ├── property/
│   │   └── user/
│   └── EstateFlowApplication.java
```

---

## Authorization Model

Roles used in the system:

* `ADMIN`
* `AGENCY_ADMIN`
* `AGENT`
* `PROPERTY_OWNER`
* `BUYER`
* `RENTER`

### Example permissions

#### ADMIN

* manage all agencies
* view platform-wide statistics
* activate or deactivate agencies
* review users and platform activity
* manage subscription plan visibility and moderation workflows

#### AGENCY_ADMIN

* manage agency data
* manage agents and owner relationships
* manage listings and promotions
* monitor inquiries, viewings, offers, and payments
* view agency reports

#### AGENT

* manage assigned listings
* respond to inquiries
* schedule and confirm viewings
* coordinate offers with buyers or renters

#### PROPERTY_OWNER

* create and manage own listings
* review inquiries, viewings, and offers
* purchase featured placements
* track listing performance and payment status

#### BUYER

* browse sale properties
* save properties
* place offers
* book viewings
* message agencies or owners

#### RENTER

* browse rental properties
* save searches and alerts
* book viewings
* send rental inquiries
* message agencies or owners

---

## Dashboard Support by Role

The backend should expose dedicated dashboard aggregation services so each role can load a compact summary without multiple expensive frontend calls.

### Admin dashboard data

Should aggregate:

* agency count
* active listing count
* user count by role
* revenue and subscription metrics
* recently flagged or suspended entities

### Agency dashboard data

Should aggregate:

* total and active listings
* leads and open inquiries
* scheduled viewings
* open offers
* team workload
* payments, commissions, and promotions

### Agent dashboard data

Should aggregate:

* assigned listings
* new inquiries
* upcoming viewings
* open offers
* overdue follow-ups

### Owner dashboard data

Should aggregate:

* owned listings
* pending inquiries
* upcoming viewings
* open offers
* featured plan payments

### Buyer dashboard data

Should aggregate:

* favorites count
* saved searches count
* recommended properties
* upcoming viewings
* offer statuses

### Renter dashboard data

Should aggregate:

* favorite rentals
* saved rental searches
* alert matches
* upcoming viewings
* inquiry activity

---
