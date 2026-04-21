# Backend Documentation

## Overview

The backend of **Nestora** is a REST API built with **Java Spring Boot**.

Its responsibilities are:

* authentication and authorization
* property listing business logic
* search, messaging, and recommendation workflows
* database operations
* file handling
* role-based access control
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

* agency-based logical multi-tenancy
* role-based user ownership
* relationships between agencies, agents, owners, clients, and properties
* favorites and saved search data
* viewing appointments, inquiries, and offers
* payment tracking, featured listings, and ads
* notifications and recommendation signals

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
* createdAt

## Property

Fields:

* id
* agency
* owner
* title
* description
* type
* listingType
* price
* status

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
* buyer
* agent
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
* user
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
* `PropertyCreateRequestDto`
* `PropertyUpdateRequestDto`
* `PropertyResponseDto`
* `SavedSearchCreateRequestDto`
* `InquiryCreateRequestDto`
* `ViewingCreateRequestDto`
* `OfferCreateRequestDto`
* `PaymentCreateRequestDto`
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
  "message": "Property not found",
  "errors": null,
  "timestamp": "2026-04-19T12:10:00"
}
```

---

## Suggested Backend Package Structure

```bash
backend/
├── src/main/java/com/nestora/
│   ├── config/
│   ├── controller/
│   ├── dto/
│   │   ├── auth/
│   │   ├── agency/
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
│   └── NestoraApplication.java
```

---

## Authorization Model

Roles used in the system:

* `SUPER_ADMIN`
* `AGENCY_ADMIN`
* `AGENT`
* `PROPERTY_OWNER`
* `BUYER`
* `RENTER`

### Example permissions

#### SUPER_ADMIN

* manage all agencies
* manage subscription plans
* see platform-wide statistics

#### AGENCY_ADMIN

* manage agency data
* manage agents and owners
* manage listings and promotions
* manage commissions and payments

#### AGENT

* manage assigned listings
* respond to inquiries
* schedule and confirm viewings

#### PROPERTY_OWNER

* create own listings
* view leads and offers
* purchase featured placements

#### BUYER

* save properties
* place offers
* book viewings and message agents

#### RENTER

* save rental searches
* book viewings
* message agents and owners

---
