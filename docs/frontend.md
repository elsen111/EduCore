# Frontend Documentation

## Overview

The frontend of **Nestora** is a modern web application built with **React** and **TypeScript**.

Its responsibilities are:

* rendering public marketplace and dashboard interfaces
* managing navigation and protected routes
* handling search, filters, and client-side validation
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
* **React Leaflet** or **Google Maps**
* **Lucide React**

---

## Frontend Responsibilities

The frontend is responsible for:

* public marketing pages
* authentication pages
* property search and listing pages
* property details and gallery pages
* favorites, recommendations, and dashboard pages
* messaging, viewing, and payment pages
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
* contact and viewing information

### Agents Page

Purpose:

* show agent cards and profiles

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

* buyer, renter, owner, or agency registration

---

## Dashboard Pages

### Dashboard Home

Purpose:

* role-based statistics
* overview cards
* charts
* quick actions

### Listings Page

Purpose:

* list owned or managed properties
* search, filter, paginate
* add, edit, archive

### Listing Details Page

Purpose:

* listing profile
* inquiry history
* viewing history
* promotion and performance data

### Listing Create / Edit Page

Purpose:

* create new property listing
* update existing property listing

### Favorites Page

Purpose:

* show saved properties
* compare and manage favorites

### Saved Searches Page

Purpose:

* manage saved filters
* enable price and availability alerts

### Messages Page

Purpose:

* manage buyer, renter, agent, and owner conversations

### Viewings Page

Purpose:

* manage property viewing requests
* confirm, reschedule, or cancel appointments

### Offers Page

Purpose:

* create and review offers
* track negotiation statuses

### Payments Page

Purpose:

* listing fee tracking
* commission history
* billing overview

### Promotions Page

Purpose:

* create featured listings
* manage ad placements
* track campaign dates

### Recommendations Page

Purpose:

* show AI-based property recommendations
* explain match reasons when available

### Notifications Page

Purpose:

* display system notifications and reminders

### Reports Page

Purpose:

* show analytics and reports
* prepare exports

### Settings Page

Purpose:

* agency or profile settings
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

* `PropertySearchForm`
* `PropertyFilterPanel`
* `PropertyForm`
* `PropertyGalleryUploader`
* `PropertyMap`
* `FavoriteToggle`
* `SavedSearchForm`
* `ViewingCalendar`
* `MessageThread`
* `PromotionForm`
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
├── /agents
├── /contact
├── /login
├── /register
├── /dashboard
│   ├── /dashboard/listings
│   ├── /dashboard/listings/create
│   ├── /dashboard/listings/:id
│   ├── /dashboard/listings/:id/edit
│   ├── /dashboard/favorites
│   ├── /dashboard/saved-searches
│   ├── /dashboard/messages
│   ├── /dashboard/viewings
│   ├── /dashboard/offers
│   ├── /dashboard/payments
│   ├── /dashboard/promotions
│   ├── /dashboard/recommendations
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
* property create/edit form
* saved search create/edit form
* viewing booking form
* offer submission form
* payment checkout form
* settings form

---

## Frontend Validation

Client-side validation should cover:

* required fields
* email format
* password strength
* min/max lengths
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
* `agencyService.ts`
* `propertyService.ts`
* `searchService.ts`
* `agentService.ts`
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

### Example roles

* `SUPER_ADMIN`
* `AGENCY_ADMIN`
* `AGENT`
* `PROPERTY_OWNER`
* `BUYER`
* `RENTER`

### Example access behavior

* Agency admin can manage agents, listings, promotions, payments
* Agent can manage assigned listings, inquiries, viewings, and offers
* Property owner can manage own listings and purchase featured placement
* Buyer can browse properties, save favorites, message agents, and place offers
* Renter can browse rentals, save searches, and schedule viewings

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
