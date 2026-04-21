# API Documentation

## Overview

This section defines the REST API structure for **Nestora**.

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

Register a new agency admin account.

### Authorization

* Public

### Request DTO

```java
public class RegisterRequestDto {
    private String agencyName;
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
  "agencyName": "Skyline Realty",
  "firstName": "Emma",
  "lastName": "Johnson",
  "email": "owner@skylinerealty.com",
  "password": "StrongPassword123",
  "phone": "+12025550123"
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
    "agencyId": 1,
    "email": "owner@skylinerealty.com"
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
  "email": "owner@skylinerealty.com",
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
    "role": "AGENCY_ADMIN"
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

## 2. Agency API

## GET `/api/agencies/me`

Get current agency data.

### Authorization

* `AGENCY_ADMIN`

### Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "Skyline Realty",
    "logoUrl": "/uploads/logo.png",
    "email": "info@skylinerealty.com",
    "phone": "+12025550123",
    "address": "Austin, Texas"
  }
}
```

---

## PUT `/api/agencies/me`

Update agency profile.

### Authorization

* `AGENCY_ADMIN`

### Request Body

```json
{
  "name": "Skyline Realty",
  "description": "Modern marketplace for buying, selling, and renting homes",
  "email": "info@skylinerealty.com",
  "phone": "+12025550123",
  "address": "Austin, Texas",
  "website": "https://skylinerealty.com"
}
```

---

## 3. Property API

## POST `/api/properties`

Create property listing.

### Authorization

* `AGENCY_ADMIN`, `AGENT`, `PROPERTY_OWNER`

### Request DTO

```java
public class PropertyCreateRequestDto {
    private String title;
    private String description;
    private String type;
    private String listingType;
    private BigDecimal price;
    private String city;
    private String address;
    private Integer bedrooms;
    private Integer bathrooms;
}
```

### Request Body

```json
{
  "title": "Modern Downtown Condo",
  "description": "2 bedroom condo with skyline view",
  "type": "CONDO",
  "listingType": "SALE",
  "price": 425000,
  "city": "Austin",
  "address": "101 Main St, Austin, TX",
  "bedrooms": 2,
  "bathrooms": 2
}
```

### Response

```json
{
  "success": true,
  "message": "Property created successfully",
  "data": {
    "id": 5,
    "title": "Modern Downtown Condo",
    "listingType": "SALE",
    "price": 425000,
    "status": "DRAFT"
  }
}
```

---

## GET `/api/properties`

Get paginated properties list.

### Authorization

* Authenticated or public depending on setup

### Query Params

* `page`
* `size`
* `search`
* `listingType`
* `city`
* `status`
* `minPrice`
* `maxPrice`

### Example

```http
GET /api/properties?page=0&size=10&city=Austin&listingType=SALE&maxPrice=500000
```

### Response

```json
{
  "success": true,
  "data": {
    "content": [
      {
        "id": 5,
        "title": "Modern Downtown Condo",
        "city": "Austin",
        "price": 425000,
        "listingType": "SALE",
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

## GET `/api/properties/{id}`

Get property details.

### Authorization

* Authenticated or public depending on setup

### Path Params

* `id`

### Response

```json
{
  "success": true,
  "data": {
    "id": 5,
    "title": "Modern Downtown Condo",
    "description": "2 bedroom condo with skyline view",
    "city": "Austin",
    "address": "101 Main St, Austin, TX",
    "price": 425000,
    "bedrooms": 2,
    "bathrooms": 2,
    "status": "ACTIVE"
  }
}
```

---

## PUT `/api/properties/{id}`

Update property listing.

### Authorization

* `AGENCY_ADMIN`, `AGENT`, `PROPERTY_OWNER`

### Path Params

* `id`

### Request Body

```json
{
  "title": "Modern Downtown Condo",
  "price": 415000,
  "status": "ACTIVE"
}
```

---

## DELETE `/api/properties/{id}`

Archive property listing.

### Authorization

* `AGENCY_ADMIN`, `PROPERTY_OWNER`

### Path Params

* `id`

### Response

```json
{
  "success": true,
  "message": "Property archived successfully"
}
```

---

## 4. Agent API

## POST `/api/agents`

Create agent.

### Authorization

* `AGENCY_ADMIN`

### Request Body

```json
{
  "firstName": "Liam",
  "lastName": "Carter",
  "email": "liam@skylinerealty.com",
  "phone": "+12025550124",
  "specialization": "Luxury Homes",
  "licenseNumber": "TX-784421"
}
```

### Response

```json
{
  "success": true,
  "message": "Agent created successfully",
  "data": {
    "id": 3,
    "firstName": "Liam",
    "lastName": "Carter"
  }
}
```

---

## GET `/api/agents`

List agents.

### Authorization

* `AGENCY_ADMIN`, `PROPERTY_OWNER`

### Query Params

* `page`
* `size`
* `search`
* `specialization`

---

## GET `/api/agents/{id}`

Get agent details.

### Authorization

* `AGENCY_ADMIN`, `PROPERTY_OWNER`, `AGENT`

---

## PUT `/api/agents/{id}`

Update agent.

### Authorization

* `AGENCY_ADMIN`

---

## DELETE `/api/agents/{id}`

Delete or deactivate agent.

### Authorization

* `AGENCY_ADMIN`

---

## 5. Saved Search API

## POST `/api/saved-searches`

Create saved search.

### Authorization

* `BUYER`, `RENTER`

### Request Body

```json
{
  "name": "Austin Condos Under 500k",
  "city": "Austin",
  "listingType": "SALE",
  "minPrice": 250000,
  "maxPrice": 500000,
  "bedrooms": 2,
  "propertyType": "CONDO",
  "isAlertEnabled": true
}
```

### Response

```json
{
  "success": true,
  "message": "Saved search created successfully",
  "data": {
    "id": 8,
    "name": "Austin Condos Under 500k",
    "isAlertEnabled": true
  }
}
```

---

## GET `/api/saved-searches`

Get saved searches list.

### Authorization

* `BUYER`, `RENTER`

### Query Params

* `page`
* `size`
* `isAlertEnabled`

---

## GET `/api/saved-searches/{id}`

Get saved search details.

### Authorization

* `BUYER`, `RENTER`

---

## PUT `/api/saved-searches/{id}`

Update saved search.

### Authorization

* `BUYER`, `RENTER`

---

## DELETE `/api/saved-searches/{id}`

Delete saved search.

### Authorization

* `BUYER`, `RENTER`

---

## 6. Favorites API

## POST `/api/favorites/{propertyId}`

Save property to favorites.

### Authorization

* `BUYER`, `RENTER`

### Path Params

* `propertyId`

### Response

```json
{
  "success": true,
  "message": "Property added to favorites"
}
```

---

## DELETE `/api/favorites/{propertyId}`

Remove property from favorites.

### Authorization

* `BUYER`, `RENTER`

### Path Params

* `propertyId`

---

## GET `/api/favorites`

List favorite properties.

### Query Params

* `page`
* `size`
* `listingType`
* `city`

---

## 7. Viewing API

## POST `/api/viewings`

Create viewing request.

### Authorization

* `BUYER`, `RENTER`, `AGENT`, `PROPERTY_OWNER`

### Request Body

```json
{
  "propertyId": 5,
  "scheduledAt": "2026-05-03T14:00:00",
  "note": "Interested in a weekend visit",
  "contactPhone": "+12025550190"
}
```

### Business Rules

* time must be valid
* archived property cannot receive viewings
* property must belong to current agency for management actions
* agent or owner cannot accept overlapping confirmed viewings

### Response

```json
{
  "success": true,
  "message": "Viewing requested successfully"
}
```

---

## GET `/api/viewings`

Get viewings.

### Query Params

* `propertyId`
* `userId`
* `status`
* `from`
* `to`

---

## 8. Inquiry API

## POST `/api/inquiries`

Create inquiry.

### Authorization

* `BUYER`, `RENTER`

### Request DTO

```java
public class InquiryCreateRequestDto {
    private Long propertyId;
    private Long recipientId;
    private String message;
    private String contactPhone;
}
```

### Request Body

```json
{
  "propertyId": 5,
  "recipientId": 3,
  "message": "Is this condo still available and can HOA fees be shared?",
  "contactPhone": "+12025550190"
}
```

### Response

```json
{
  "success": true,
  "message": "Inquiry sent successfully"
}
```

---

## GET `/api/inquiries/property/{propertyId}`

Get property inquiries.

### Authorization

* `AGENCY_ADMIN`, `AGENT`, `PROPERTY_OWNER`

### Query Params

* `from`
* `to`
* `status`

---

## 9. Payment API

## POST `/api/payments`

Create payment record.

### Authorization

* `AGENCY_ADMIN`, `PROPERTY_OWNER`

### Request Body

```json
{
  "propertyId": 5,
  "purpose": "FEATURED_LISTING",
  "amount": 199,
  "provider": "STRIPE",
  "paymentMethod": "CARD",
  "status": "PAID",
  "note": "Featured listing for 14 days"
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

* `userId`
* `purpose`
* `status`
* `from`
* `to`
* `page`
* `size`

---

## GET `/api/payments/me`

Get current user payment history.

---

## 10. Messaging API

## POST `/api/messages/conversations`

Create conversation.

### Authorization

* `BUYER`, `RENTER`, `AGENT`, `PROPERTY_OWNER`

### Request Body

```json
{
  "propertyId": 5,
  "recipientId": 3,
  "initialMessage": "Hi, I would like to discuss this property."
}
```

---

## POST `/api/messages/conversations/{id}/send`

Send message.

### Authorization

* `BUYER`, `RENTER`, `AGENT`, `PROPERTY_OWNER`

### Content-Type

* `multipart/form-data`

### Path Params

* `id`

### Form Data

* `text`
* `attachment`

### Response

```json
{
  "success": true,
  "message": "Message sent successfully"
}
```

---

## GET `/api/messages/conversations/{id}`

List messages for conversation.

---

## 11. Offer API

## POST `/api/offers`

Create offer.

### Authorization

* `BUYER`

### Request Body

```json
{
  "propertyId": 5,
  "amount": 410000,
  "financingType": "MORTGAGE",
  "message": "Offer contingent on inspection"
}
```

---

## PUT `/api/offers/{id}/status`

Update offer status.

### Authorization

* `AGENCY_ADMIN`, `AGENT`, `PROPERTY_OWNER`

### Request Body

```json
{
  "status": "COUNTERED",
  "counterAmount": 420000
}
```

### Response

```json
{
  "success": true,
  "message": "Offer status updated successfully"
}
```

---

## GET `/api/offers/property/{propertyId}`

List offers for property.

---

## 12. Promotion API

## POST `/api/promotions`

Create promotion.

### Authorization

* `AGENCY_ADMIN`, `AGENT`, `PROPERTY_OWNER`

### Request Body

```json
{
  "propertyId": 5,
  "planType": "FEATURED_14_DAYS",
  "startDate": "2026-05-01",
  "endDate": "2026-05-14",
  "budget": 199
}
```

---

## POST `/api/promotions/upload-banner`

Upload banner asset.

### Authorization

* `AGENCY_ADMIN`

### Content-Type

* `multipart/form-data`

### Form Data

* `promotionId`
* `image`
* `targetUrl`

---

## GET `/api/promotions/property/{propertyId}`

List promotions by property.

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
      "title": "Viewing Reminder",
      "message": "Your viewing is scheduled for tomorrow at 2:00 PM",
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

## GET `/api/dashboard/agency`

Get agency dashboard statistics.

### Authorization

* `AGENCY_ADMIN`

### Response

```json
{
  "success": true,
  "data": {
    "totalListings": 120,
    "activeAgents": 12,
    "featuredListings": 15,
    "monthlyRevenue": 8400,
    "openInquiries": 27,
    "scheduledViewings": 18
  }
}
```

---

## GET `/api/dashboard/agent`

Get agent dashboard data.

### Authorization

* `AGENT`

---

## GET `/api/dashboard/client`

Get buyer or renter dashboard data.

### Authorization

* `BUYER`, `RENTER`

---

## 15. Reports API

## GET `/api/reports/listings`

Get listing performance report.

### Authorization

* `AGENCY_ADMIN`

### Query Params

* `status`
* `city`
* `listingType`
* `from`
* `to`

---

## GET `/api/reports/payments`

Get payments report.

### Authorization

* `AGENCY_ADMIN`

### Query Params

* `status`
* `purpose`
* `from`
* `to`

---

## GET `/api/reports/conversions`

Get inquiry and offer conversion report.

### Authorization

* `AGENCY_ADMIN`, `AGENT`

---

## Example Development Order for API

### Phase 1

* auth
* agency
* properties
* agents

### Phase 2

* saved searches
* favorites
* viewings
* inquiries

### Phase 3

* payments
* messaging
* offers
* promotions

### Phase 4

* notifications
* dashboard
* reports
