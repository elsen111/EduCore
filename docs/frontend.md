# Frontend Documentation

## Overview

The frontend of **EstateFlow** is a modern web application built with **React** and **TypeScript**.

Its responsibilities are:

* rendering public marketplace and role-based dashboard interfaces
* managing navigation, route guards, and role redirects
* handling property search, filters, forms, and client-side validation
* connecting to backend APIs
* displaying role-aware content for admin, agency, owner, buyer, and renter users
* supporting forgot-password with OTP, password reset, and change-password flows
* supporting responsive design
* managing UI state, authentication state, and notification state

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
* **React Leaflet** or **Google Maps**
* **Lucide React**

---

## Frontend Responsibilities

The frontend is responsible for:

* public marketing pages
* authentication and account onboarding pages
* forgot-password with OTP verification pages
* reset-password and change-password pages
* property search and listing pages
* property details and gallery pages
* favorites, saved searches, and recommendation pages
* messaging, inquiry, viewing, payment, and offer pages
* admin, agency, owner, buyer, and renter dashboard pages
* role-based UI rendering and navigation
* API request and response handling
* token storage, refresh handling, and route protection
* reusable component system

---

## Frontend Architecture

The frontend should follow a modular and scalable structure.

### Main architectural ideas

* reusable components
* feature-based page separation
* service-based API communication
* typed request and response handling
* context-based auth, dashboard, and notification state
* protected dashboard routes
* role-aware navigation menus and dashboard landing pages
* validation schemas separated from UI

---

## User Roles and Experience Model

The frontend should provide a tailored dashboard and navigation experience for each primary role.

| Role | Primary experience | Default dashboard route |
| --- | --- | --- |
| `ADMIN` | platform operations, agency oversight, user management, billing visibility | `/dashboard/admin` |
| `AGENCY_ADMIN` | agency operations, team management, listings, leads, reports | `/dashboard/agency` |
| `PROPERTY_OWNER` | owner self-service listing management, leads, offers, payments | `/dashboard/owner` |
| `BUYER` | property discovery, favorites, viewings, offers, messaging | `/dashboard/buyer` |
| `RENTER` | rental discovery, saved searches, alerts, viewings, messaging | `/dashboard/renter` |
| `AGENT` | optional internal agency role for daily sales operations | `/dashboard/agent` |

The dashboard entry page at `/dashboard` should redirect the user to the correct dashboard home based on `user.role`.

---

## Frontend Pages

## Public Pages

### Home Page

Purpose:

* introduce the platform
* explain benefits for agencies, owners, buyers, and renters
* display featured properties
* provide call-to-action buttons

### About Page

Purpose:

* show company or platform information
* mission and vision
* short team or business intro

### Properties Page

Purpose:

* display all available properties
* search, filter, sort, and explore on map

### Property Details Page

Purpose:

* show single property details
* image gallery
* price and location
* amenities
* agency or owner profile summary
* contact, inquiry, and viewing actions

### Agencies or Agents Page

Purpose:

* show agency cards, agent cards, and profiles

### Contact Page

Purpose:

* contact form
* address
* phone
* email
* map section

---

## Auth and Security Pages

### Login Page

Purpose:

* authenticate users
* redirect user based on role
* display account status or lockout messages when needed

### Register Page

Purpose:

* allow buyer, renter, owner, or agency account registration
* collect role-specific onboarding fields

### Forgot Password Page

Purpose:

* collect email or phone
* send OTP for password recovery
* explain resend cooldown and OTP expiry

### OTP Verification Page

Purpose:

* verify the OTP code sent during password recovery
* allow resend when cooldown expires
* block further progress until OTP is valid

### Reset Password Page

Purpose:

* allow a user with verified OTP to set a new password
* confirm password match
* display password policy requirements

### Change Password Page

Purpose:

* allow authenticated users to change password from settings
* require current password
* optionally force logout from other sessions after password change

---

## Dashboard Pages

### Dashboard Shell

Purpose:

* shared sidebar and topbar
* role-aware menu items
* notifications tray
* quick search and quick action entry points

### Shared Dashboard Pages

These pages can exist for multiple roles, but the visible widgets and actions should vary by role.

#### Messages Page

Purpose:

* manage buyer, renter, owner, agent, and agency conversations

#### Notifications Page

Purpose:

* display system notifications and reminders

#### Viewings Page

Purpose:

* manage property viewing requests
* confirm, reschedule, or cancel appointments

#### Settings Page

Purpose:

* profile settings
* notification preferences
* branding where applicable
* connected contact details

#### Security Page

Purpose:

* change password
* show password last changed date
* manage device sessions if implemented

---

## Role-Based Dashboard Pages

### Admin Dashboard Pages

#### Admin Home

Purpose:

* platform-wide statistics
* active agencies
* active users by role
* flagged listings or incidents
* revenue and subscription overview

#### Agencies Management Page

Purpose:

* list all agencies
* activate, suspend, or review accounts
* monitor plan usage and onboarding status

#### Users and Roles Page

Purpose:

* search users by role
* inspect account status
* support role corrections or deactivation workflows

#### Platform Reports Page

Purpose:

* platform growth
* listing supply by region
* transaction funnel summaries
* exportable operational reports

#### Platform Settings Page

Purpose:

* subscription plan settings
* moderation rules
* featured listing pricing

### Agency Dashboard Pages

#### Agency Home

Purpose:

* role-based statistics
* active listings
* new leads
* upcoming viewings
* open offers
* quick actions for staff and listing management

#### Listings Page

Purpose:

* list managed properties
* search, filter, paginate
* add, edit, publish, archive

#### Listing Details Page

Purpose:

* listing profile
* inquiry history
* viewing history
* promotion and performance data

#### Listing Create or Edit Page

Purpose:

* create new property listing
* update existing property listing

#### Team Management Page

Purpose:

* manage agents and owner relationships
* assign listings to staff
* review performance and workload

#### Leads and Inquiries Page

Purpose:

* review incoming buyer and renter interest
* update inquiry status
* assign follow-up responsibility

#### Offers Pipeline Page

Purpose:

* review offers by property
* update negotiation statuses
* monitor conversion pipeline

#### Payments and Commissions Page

Purpose:

* listing fee tracking
* commission history
* billing overview

#### Promotions Page

Purpose:

* create featured listings
* manage ad placements
* track campaign dates

#### Reports Page

Purpose:

* listing analytics
* agent performance
* inquiry and offer conversion metrics

### Agent Dashboard Pages

#### Agent Home

Purpose:

* assigned listings
* new inquiries
* upcoming viewings
* open offers
* quick follow-up tasks

#### Assigned Listings Page

Purpose:

* manage assigned properties
* update listing details
* coordinate with agency admin and owners

#### Agent Leads Page

Purpose:

* respond to buyer and renter inquiries
* update lead stages
* prepare viewing follow-ups

#### Agent Calendar and Offers Page

Purpose:

* manage viewing appointments
* review open offers on assigned listings
* keep negotiation workflow moving

### Owner Dashboard Pages

#### Owner Home

Purpose:

* owned property count
* active listings
* pending inquiries
* upcoming viewings
* open offers
* payment summary

#### My Properties Page

Purpose:

* show owned properties
* create and edit listings
* track publishing status

#### Owner Leads Page

Purpose:

* review inquiries and viewing requests
* see response status
* coordinate with assigned agency or agent when applicable

#### Owner Offers Page

Purpose:

* review purchase offers
* accept, reject, or counter
* view negotiation timeline

#### Owner Payments and Promotions Page

Purpose:

* track featured listing purchases
* see invoices and payment status
* monitor campaign performance

### Buyer Dashboard Pages

#### Buyer Home

Purpose:

* favorite count
* saved searches
* recommended properties
* upcoming viewings
* open offers

#### Favorites Page

Purpose:

* show saved properties
* compare and manage favorites

#### Saved Searches Page

Purpose:

* manage saved filters
* enable price and availability alerts

#### Buyer Viewings Page

Purpose:

* request property viewings
* track confirmation status
* reschedule or cancel when allowed

#### Buyer Offers Page

Purpose:

* create and review offers
* track negotiation statuses

#### Recommendations Page

Purpose:

* show recommendation-based property suggestions
* explain match reasons when available

### Renter Dashboard Pages

#### Renter Home

Purpose:

* favorite rentals
* saved rental searches
* new listing alerts
* upcoming viewings
* inquiry status

#### Rental Favorites Page

Purpose:

* manage favorite rental properties

#### Rental Saved Searches Page

Purpose:

* manage rental filters and alerts

#### Renter Viewings Page

Purpose:

* request and manage rental viewing appointments

#### Rental Inquiries Page

Purpose:

* track messages and inquiry status for rental properties

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
* `AuthLayout`
* `DashboardLayout`
* `Sidebar`
* `Topbar`
* `Footer`
* `ProtectedRoute`
* `RoleRedirect`

## Feature Components

* `PropertySearchForm`
* `PropertyFilterPanel`
* `PropertyForm`
* `PropertyGalleryUploader`
* `PropertyMap`
* `FavoriteToggle`
* `SavedSearchForm`
* `ViewingCalendar`
* `MessageThread`
* `OfferForm`
* `PromotionForm`
* `OtpInput`
* `PasswordStrengthMeter`
* `DashboardStatsCards`
* `RoleDashboardSwitcher`

---

## Frontend State Management

Recommended approach:

* **React Context** for auth and app-wide settings
* local state for page-specific UI logic
* optional **Zustand** or **Redux Toolkit** if project grows larger

### Suggested contexts

* `AuthContext`
* `UserContext`
* `DashboardContext`
* `SearchContext`
* `ThemeContext`
* `NotificationContext`

---

## Frontend Routing

Example route structure:

```txt
/
├── /
├── /about
├── /properties
├── /properties/:id
├── /agencies
├── /agents
├── /contact
├── /login
├── /register
├── /forgot-password
├── /forgot-password/verify-otp
├── /reset-password
├── /dashboard
│   ├── /dashboard/admin
│   ├── /dashboard/admin/agencies
│   ├── /dashboard/admin/users
│   ├── /dashboard/admin/reports
│   ├── /dashboard/agency
│   ├── /dashboard/agency/listings
│   ├── /dashboard/agency/listings/create
│   ├── /dashboard/agency/listings/:id
│   ├── /dashboard/agency/listings/:id/edit
│   ├── /dashboard/agency/team
│   ├── /dashboard/agency/leads
│   ├── /dashboard/agency/viewings
│   ├── /dashboard/agency/offers
│   ├── /dashboard/agency/payments
│   ├── /dashboard/agency/promotions
│   ├── /dashboard/agency/reports
│   ├── /dashboard/agent
│   ├── /dashboard/agent/listings
│   ├── /dashboard/agent/leads
│   ├── /dashboard/agent/viewings
│   ├── /dashboard/agent/offers
│   ├── /dashboard/owner
│   ├── /dashboard/owner/properties
│   ├── /dashboard/owner/leads
│   ├── /dashboard/owner/offers
│   ├── /dashboard/owner/payments
│   ├── /dashboard/buyer
│   ├── /dashboard/buyer/favorites
│   ├── /dashboard/buyer/saved-searches
│   ├── /dashboard/buyer/viewings
│   ├── /dashboard/buyer/offers
│   ├── /dashboard/buyer/recommendations
│   ├── /dashboard/renter
│   ├── /dashboard/renter/favorites
│   ├── /dashboard/renter/saved-searches
│   ├── /dashboard/renter/viewings
│   ├── /dashboard/renter/inquiries
│   ├── /dashboard/messages
│   ├── /dashboard/notifications
│   ├── /dashboard/settings
│   └── /dashboard/security
```

---

## Frontend Form Handling

Recommended libraries:

* **React Hook Form**
* **Zod**

Used for:

* login form
* register form
* forgot-password form
* OTP verification form
* reset-password form
* change-password form
* profile settings form
* property create or edit form
* saved search create or edit form
* viewing booking form
* offer submission form
* payment checkout form

---

## Frontend Validation

Client-side validation should cover:

* required fields
* email format
* password strength
* password confirmation match
* current password requirement for change-password
* OTP length and numeric format
* resend cooldown display
* min and max lengths
* valid numbers
* valid dates
* image type validation
* image size validation
* price range and map coordinate validation before submission if needed

---

## Frontend API Integration

All API calls should be separated into service files.

### Suggested services

* `authService.ts`
* `passwordService.ts`
* `profileService.ts`
* `adminService.ts`
* `agencyService.ts`
* `agentService.ts`
* `ownerService.ts`
* `propertyService.ts`
* `searchService.ts`
* `favoriteService.ts`
* `savedSearchService.ts`
* `viewingService.ts`
* `offerService.ts`
* `messageService.ts`
* `paymentService.ts`
* `promotionService.ts`
* `recommendationService.ts`
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

### Primary roles

* `ADMIN`
* `AGENCY_ADMIN`
* `AGENT`
* `PROPERTY_OWNER`
* `BUYER`
* `RENTER`

### Example access behavior

* Admin can manage agencies, users, subscriptions, platform reports, and moderation workflows
* Agency admin can manage team members, listings, promotions, payments, viewings, and reports
* Agent can manage assigned listings, inquiries, viewings, and offers
* Property owner can manage own listings, leads, offers, and featured placement payments
* Buyer can browse sale properties, save favorites, message agencies or owners, schedule viewings, and place offers
* Renter can browse rental properties, save searches, receive alerts, message agencies or owners, and track viewing activity

### Example protected route

```tsx
type ProtectedRouteProps = {
  roles?: string[];
  children: React.ReactNode;
};

function ProtectedRoute({ roles, children }: ProtectedRouteProps) {
  const user = useAuth();

  if (!user) return <Navigate to="/login" replace />;
  if (roles && !roles.includes(user.role)) return <Navigate to="/dashboard" replace />;

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
│   │   ├── auth/
│   │   ├── layout/
│   │   └── dashboard/
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
