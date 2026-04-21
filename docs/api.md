# API Documentation

## Overview

This section defines the REST API structure for **EstateFlow**.

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

## Role Coverage Overview

The API should support the following primary role journeys.

| Role | Main dashboard endpoint | Common supporting endpoints |
| --- | --- | --- |
| `ADMIN` | `GET /api/dashboard/admin` | `/api/admin/agencies`, `/api/admin/users`, `/api/reports/platform` |
| `AGENCY_ADMIN` | `GET /api/dashboard/agency` | `/api/agencies/me`, `/api/agents`, `/api/owners`, `/api/properties`, `/api/viewings`, `/api/offers`, `/api/promotions` |
| `PROPERTY_OWNER` | `GET /api/dashboard/owner` | `/api/users/me`, `/api/properties`, `/api/inquiries/property/{propertyId}`, `/api/offers/property/{propertyId}`, `/api/payments/me` |
| `BUYER` | `GET /api/dashboard/buyer` | `/api/favorites`, `/api/saved-searches`, `/api/viewings`, `/api/offers`, `/api/messages/conversations` |
| `RENTER` | `GET /api/dashboard/renter` | `/api/favorites`, `/api/saved-searches`, `/api/viewings`, `/api/inquiries/me`, `/api/messages/conversations` |
| `AGENT` | `GET /api/dashboard/agent` | `/api/agents/{id}`, `/api/properties`, `/api/viewings`, `/api/offers/property/{propertyId}` |

---

## 1. Auth API

## POST `/api/auth/register`

Register a new account for agency admin, property owner, buyer, or renter.

### Authorization

* Public

### Request DTO

```java
public class RegisterRequestDto {
    private String role;
    private String agencyName;
    private String firstName;
    private String lastName;
    private String email;
    private String password;
    private String phone;
    private String inviteCode;
}
```

### Business Rules

* `AGENCY_ADMIN` requires `agencyName`
* `PROPERTY_OWNER` may optionally use `inviteCode` if onboarding under an agency
* `BUYER` and `RENTER` do not require agency fields
* password must satisfy platform password policy

### Example Request Body

```json
{
  "role": "AGENCY_ADMIN",
  "agencyName": "Skyline Realty",
  "firstName": "Emma",
  "lastName": "Johnson",
  "email": "owner@skylinerealty.com",
  "password": "StrongPassword123!",
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
    "role": "AGENCY_ADMIN",
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
  "password": "StrongPassword123!"
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

## GET `/api/auth/me`

Return the currently authenticated user and role context.

### Authorization

* Authenticated user

### Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "firstName": "Emma",
    "lastName": "Johnson",
    "email": "owner@skylinerealty.com",
    "role": "AGENCY_ADMIN",
    "agencyId": 1
  }
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

## POST `/api/auth/forgot-password`

Start the forgot-password flow and send OTP to the user.

### Authorization

* Public

### Request DTO

```java
public class ForgotPasswordRequestDto {
    private String email;
    private String deliveryChannel;
}
```

### Request Body

```json
{
  "email": "buyer@estateflow.com",
  "deliveryChannel": "EMAIL"
}
```

### Responses

#### `200 OK`

```json
{
  "success": true,
  "message": "OTP sent successfully",
  "data": {
    "otpExpiresInSeconds": 300,
    "maskedDestination": "b***@estateflow.com"
  }
}
```

#### `404 Not Found`

```json
{
  "success": false,
  "message": "Account not found"
}
```

---

## POST `/api/auth/forgot-password/verify-otp`

Verify the OTP code before allowing password reset.

### Authorization

* Public

### Request DTO

```java
public class VerifyOtpRequestDto {
    private String email;
    private String otp;
}
```

### Request Body

```json
{
  "email": "buyer@estateflow.com",
  "otp": "482913"
}
```

### Response

```json
{
  "success": true,
  "message": "OTP verified successfully",
  "data": {
    "resetToken": "password-reset-token"
  }
}
```

---

## POST `/api/auth/forgot-password/resend-otp`

Resend a new OTP when cooldown rules allow it.

### Authorization

* Public

### Request Body

```json
{
  "email": "buyer@estateflow.com"
}
```

### Response

```json
{
  "success": true,
  "message": "OTP resent successfully"
}
```

---

## POST `/api/auth/reset-password`

Reset password after successful OTP verification.

### Authorization

* Public with verified reset token

### Request DTO

```java
public class ResetPasswordRequestDto {
    private String resetToken;
    private String newPassword;
    private String confirmPassword;
}
```

### Request Body

```json
{
  "resetToken": "password-reset-token",
  "newPassword": "NewStrongPassword123!",
  "confirmPassword": "NewStrongPassword123!"
}
```

### Response

```json
{
  "success": true,
  "message": "Password reset successfully"
}
```

---

## PUT `/api/auth/change-password`

Change password for an authenticated user from the security settings page.

### Authorization

* Authenticated user

### Request DTO

```java
public class ChangePasswordRequestDto {
    private String currentPassword;
    private String newPassword;
    private String confirmPassword;
    private boolean logoutOtherSessions;
}
```

### Request Body

```json
{
  "currentPassword": "StrongPassword123!",
  "newPassword": "EvenStrongerPassword456!",
  "confirmPassword": "EvenStrongerPassword456!",
  "logoutOtherSessions": true
}
```

### Response

```json
{
  "success": true,
  "message": "Password changed successfully"
}
```

---

## 2. Profile API

## GET `/api/users/me`

Get current user profile data for settings pages.

### Authorization

* Authenticated user

### Response

```json
{
  "success": true,
  "data": {
    "id": 8,
    "firstName": "Olivia",
    "lastName": "Reed",
    "email": "buyer@estateflow.com",
    "phone": "+12025550190",
    "role": "BUYER"
  }
}
```

---

## PUT `/api/users/me`

Update current user profile settings.

### Authorization

* Authenticated user

### Request Body

```json
{
  "firstName": "Olivia",
  "lastName": "Reed",
  "phone": "+12025550190"
}
```

---

## 3. Admin API

## GET `/api/admin/agencies`

List agencies for platform administration.

### Authorization

* `ADMIN`

### Query Params

* `page`
* `size`
* `search`
* `status`
* `subscriptionPlan`

---

## PUT `/api/admin/agencies/{id}/status`

Activate, suspend, or review an agency account.

### Authorization

* `ADMIN`

### Path Params

* `id`

### Request Body

```json
{
  "status": "SUSPENDED",
  "reason": "Subscription payment overdue"
}
```

---

## GET `/api/admin/users`

List platform users by role or status.

### Authorization

* `ADMIN`

### Query Params

* `page`
* `size`
* `search`
* `role`
* `isActive`

---

## 4. Agency API

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
    "address": "Austin, Texas",
    "subscriptionPlan": "PRO"
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

## 5. Agent API

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

## 6. Owner API

## POST `/api/owners`

Create or invite a property owner record under an agency.

### Authorization

* `AGENCY_ADMIN`

### Request Body

```json
{
  "firstName": "Noah",
  "lastName": "Turner",
  "email": "owner@skylinehomes.com",
  "phone": "+12025550180",
  "notes": "Portfolio owner with 3 active listings"
}
```

---

## GET `/api/owners`

List owners managed by the current agency.

### Authorization

* `AGENCY_ADMIN`

### Query Params

* `page`
* `size`
* `search`

---

## GET `/api/owners/{id}`

Get owner details.

### Authorization

* `AGENCY_ADMIN`, `PROPERTY_OWNER`

---

## PUT `/api/owners/{id}`

Update owner profile or agency notes.

### Authorization

* `AGENCY_ADMIN`, `PROPERTY_OWNER`

---

## 7. Property API

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
* `agencyId`
* `ownerId`
* `assignedAgentId`

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

## 8. Saved Search API

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

## 9. Favorites API

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

### Authorization

* `BUYER`, `RENTER`

### Query Params

* `page`
* `size`
* `listingType`
* `city`

---

## 10. Viewing API

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

### Authorization

* Authenticated users with role-based filtering

### Query Params

* `propertyId`
* `userId`
* `status`
* `from`
* `to`

---

## PUT `/api/viewings/{id}/status`

Confirm, reschedule, complete, or cancel a viewing.

### Authorization

* `AGENCY_ADMIN`, `AGENT`, `PROPERTY_OWNER`

### Path Params

* `id`

### Request Body

```json
{
  "status": "CONFIRMED",
  "scheduledAt": "2026-05-03T14:30:00",
  "note": "Updated after phone confirmation"
}
```

---

## 11. Inquiry API

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

## GET `/api/inquiries/me`

Get inquiries created by the current buyer or renter.

### Authorization

* `BUYER`, `RENTER`

### Query Params

* `status`
* `page`
* `size`

---

## PUT `/api/inquiries/{id}/status`

Update inquiry status.

### Authorization

* `AGENCY_ADMIN`, `AGENT`, `PROPERTY_OWNER`

### Request Body

```json
{
  "status": "CONTACTED"
}
```

---

## 12. Payment API

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

### Authorization

* `ADMIN`, `AGENCY_ADMIN`

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

### Authorization

* `AGENCY_ADMIN`, `PROPERTY_OWNER`

---

## 13. Messaging API

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

## GET `/api/messages/conversations`

List current user conversations.

### Authorization

* Authenticated users involved in messaging

### Query Params

* `propertyId`
* `status`
* `page`
* `size`

---

## GET `/api/messages/conversations/{id}`

List messages for conversation.

### Authorization

* Conversation participants only

---

## POST `/api/messages/conversations/{id}/send`

Send message.

### Authorization

* Conversation participants only

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

## 14. Offer API

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

## GET `/api/offers/me`

Get offers created by the current buyer.

### Authorization

* `BUYER`

### Query Params

* `status`
* `page`
* `size`

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

### Authorization

* `AGENCY_ADMIN`, `AGENT`, `PROPERTY_OWNER`

---

## 15. Promotion API

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

### Authorization

* `AGENCY_ADMIN`, `AGENT`, `PROPERTY_OWNER`

---

## 16. Notification API

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

## 17. Dashboard API

## GET `/api/dashboard/admin`

Get admin dashboard statistics.

### Authorization

* `ADMIN`

### Response

```json
{
  "success": true,
  "data": {
    "totalAgencies": 42,
    "activeListings": 1280,
    "usersByRole": {
      "AGENCY_ADMIN": 42,
      "PROPERTY_OWNER": 185,
      "BUYER": 670,
      "RENTER": 390
    },
    "monthlyRevenue": 18400,
    "suspendedAgencies": 2
  }
}
```

---

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
    "activeOwners": 28,
    "featuredListings": 15,
    "monthlyRevenue": 8400,
    "openInquiries": 27,
    "scheduledViewings": 18,
    "openOffers": 11
  }
}
```

---

## GET `/api/dashboard/owner`

Get owner dashboard data.

### Authorization

* `PROPERTY_OWNER`

### Response

```json
{
  "success": true,
  "data": {
    "ownedListings": 6,
    "activeListings": 4,
    "pendingInquiries": 9,
    "upcomingViewings": 3,
    "openOffers": 2,
    "latestPaymentStatus": "PAID"
  }
}
```

---

## GET `/api/dashboard/buyer`

Get buyer dashboard data.

### Authorization

* `BUYER`

### Response

```json
{
  "success": true,
  "data": {
    "favoritesCount": 12,
    "savedSearchesCount": 3,
    "recommendedPropertiesCount": 8,
    "upcomingViewings": 2,
    "activeOffers": 1
  }
}
```

---

## GET `/api/dashboard/renter`

Get renter dashboard data.

### Authorization

* `RENTER`

### Response

```json
{
  "success": true,
  "data": {
    "favoriteRentalsCount": 7,
    "savedSearchesCount": 4,
    "newAlertMatches": 5,
    "upcomingViewings": 1,
    "openInquiries": 3
  }
}
```

---

## GET `/api/dashboard/agent`

Get agent dashboard data.

### Authorization

* `AGENT`

### Response

```json
{
  "success": true,
  "data": {
    "assignedListings": 18,
    "newInquiries": 6,
    "upcomingViewings": 4,
    "openOffers": 3
  }
}
```

---

## 18. Reports API

## GET `/api/reports/platform`

Get platform-level report data.

### Authorization

* `ADMIN`

### Query Params

* `from`
* `to`
* `groupBy`

---

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

## 19. Example Development Order for API

### Phase 1

* auth
* forgot-password OTP
* reset-password
* change-password
* profile

### Phase 2

* agency
* agents
* owners
* properties

### Phase 3

* saved searches
* favorites
* inquiries
* viewings
* messaging
* offers

### Phase 4

* payments
* promotions
* notifications
* dashboards
* reports
