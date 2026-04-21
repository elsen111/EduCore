# EstateFlow — Real Estate Marketplace and Property Platform

A full-stack real estate SaaS platform built for agencies, brokers, property owners, buyers, and renters.

EstateFlow helps users buy, sell, and rent properties through searchable listings, image-rich property pages, messaging, viewing requests, payments, promotions, and analytics in one centralized system.

The project combines a modern frontend built with React and a planned backend architecture built with Java Spring Boot.

The current repository contains the frontend scaffold and project documentation. The Spring Boot backend described below represents the intended full-stack architecture for the complete version of the project.

---

## 🚀 Project Purpose

Many real estate businesses still manage listings, inquiries, appointments, and payments manually using spreadsheets, social media posts, phone calls, and messaging apps.

EstateFlow aims to digitize the full workflow of a real estate platform by providing:

* Property listing management
* Property search and filtering
* Image-rich listing previews
* Map-based property exploration
* Favorites and saved searches
* Viewing request management
* Messaging and inquiries
* Offer tracking
* Payment and monetization tools
* Reports and analytics
* Public website pages

---

## 🎯 Main Goals

* Centralize property buying, selling, and renting workflows in one platform
* Reduce manual work and missed leads
* Improve communication between agencies, agents, owners, buyers, and renters
* Provide detailed reports, analytics, and recommendation-ready architecture
* Support multiple user roles
* Create a scalable SaaS and marketplace architecture for future business growth

---

## 👥 User Roles

### 1. Super Admin

The system owner who manages the whole platform.

#### Permissions

* Manage all agencies
* View platform-wide statistics
* Activate or deactivate agency accounts
* Manage subscription plans
* Control system settings
* Monitor all users

### 2. Agency Owner / Admin

The agency owner or manager who uses the platform.

#### Permissions

* Manage agency profile
* Add agents and staff
* Create and manage property listings
* Manage promotions and featured listings
* Track commissions and payments
* View reports
* Manage agency accounts

### 3. Agent

#### Permissions

* View assigned properties
* Update listing details
* Respond to inquiries
* Schedule viewings
* Track offers
* Communicate with buyers and renters

### 4. Property Owner

#### Permissions

* Create and manage own listings
* Upload property photos
* View inquiries and offer activity
* Purchase featured listing plans
* Track listing performance
* Monitor payment status

### 5. Buyer

#### Permissions

* Browse sale properties
* Search and filter listings
* Save favorite properties
* Book viewings
* Message agents
* Place offers

### 6. Renter

#### Permissions

* Browse rental properties
* Save searches
* Receive listing alerts
* Book viewings
* Message agents or owners
* Track rental inquiries

---

## 🧩 Main Modules

### Authentication Module

#### Features

* Register agency or user account
* Login and logout
* Forgot password
* Reset password
* JWT authentication
* Refresh tokens
* Role-based access control
* Protected routes

---

### Agency Profile Module

#### Features

* Agency name
* Logo upload
* Description
* Contact information
* Address
* Social media links
* Working hours
* Branch management
* Subscription plan details

---

### Property Listing Management Module

#### Features

* Add, edit, delete property listings
* Property details page
* Property image gallery
* Property type and listing type
* Price and status
* Amenities and features
* Address and map coordinates
* Assigned agent
* Listing status and visibility
* Notes and comments
* Listing search and filtering

---

### Agent Management Module

#### Features

* Add, edit, delete agents
* Agent profile
* Assigned listings
* Specialization information
* License information
* Available hours
* Performance notes
* Lead and inquiry tracking

---

### Property Search Module

#### Features

* Search by keyword
* Filter by city, type, and status
* Price range filtering
* Bedroom and bathroom filters
* Property size filters
* Map-based exploration
* Sort by newest, price, or popularity
* AI recommendation-ready search logic

#### Example Properties

* Downtown Luxury Condo
* Family Villa with Garden
* Furnished Studio for Rent
* Office Space in Business Center

---

### Favorites / Saved Searches Module

#### Features

* Save favorite properties
* Create saved searches
* Price alert preferences
* Availability alerts
* Compare selected properties
* Quick access to recently viewed listings

---

### Viewing / Scheduling Module

#### Features

* Request property viewing
* Calendar-based appointment management
* Agent confirmation and rescheduling
* Viewing notes
* Conflict detection
* Prevent overlapping confirmed viewings
* Online meeting links for virtual tours
* Calendar view

---

### Messaging / Inquiry Module

#### Features

* Send inquiry about a property
* Property-based conversations
* Buyer and renter messaging
* Agent and owner replies
* Attachment support
* Conversation history
* Lead status tracking

---

### Payment / Monetization Module

#### Features

* Paid listing tracking
* Featured listing purchases
* Commission records
* Ad placement billing
* Invoice generation
* Payment reminders
* Payment history

---

### Offer / Negotiation Module

#### Features

* Create offers
* Counter-offer support
* Offer status history
* Accept or reject offers
* Buyer offer tracking
* Negotiation notes
* Downloadable offer summaries

---

### Notification Module

#### Features

* In-app notifications
* Inquiry reminders
* Viewing reminders
* Payment reminders
* Offer updates
* Listing status updates

---

### Dashboard & Analytics Module

#### Agency Dashboard

* Total listings
* Total agents
* Featured listings
* Monthly revenue
* Open inquiries
* Scheduled viewings
* Active promotions

#### Agent Dashboard

* Assigned listings
* New inquiries
* Upcoming viewings
* Open offers

#### Buyer / Renter Dashboard

* Favorite properties
* Saved searches
* Upcoming viewings
* Inquiry history
* Offer status

---

### Reports Module

#### Features

* Listing performance reports
* Payment reports
* Inquiry conversion reports
* Agent workload reports
* Property popularity reports
* Export to PDF
* Export to Excel

---

### Public Website Module

#### Public Pages

* Home page
* About page
* Properties page
* Property details page
* Agents page
* Contact page
* Registration page
* Login page

---

### Current Features

* React + TypeScript + Vite frontend scaffold
* Tailwind-ready frontend setup
* Frontend service, route, hook, context, schema, and type folders
* Root project documentation for frontend, backend, and API planning
* Real estate domain documentation for listings, messaging, payments, and dashboards

### Additional Features (In the future)

* Full Spring Boot backend implementation
* Property image uploads and gallery management
* Advanced map integration
* AI property recommendations
* Real-time messaging
* Offer and negotiation workflow
* Payment gateway integration
* Featured listings and ad placements

---

## 🛠️ Tech Stack

### Frontend

* React
* TypeScript
* Tailwind CSS
* React Router
* Axios
* React Helmet Async
* i18n
* Recharts
* React Hook Form (planned)
* Zod (planned)
* React Leaflet or Google Maps (planned)

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
* Payment Integration

### Deployment

* Docker
* Nginx
* VPS Hosting
* PostgreSQL Server
* Cloud Storage or CDN for media later

---

## 🗄️ Database Entities

Main database tables:

* users
* roles
* agencies
* agents
* property_owners
* properties
* property_images
* property_features
* favorites
* saved_searches
* inquiries
* conversations
* messages
* viewings
* offers
* payments
* promotions
* ad_placements
* notifications

---

## 📁 Suggested Project Structure

```bash
docs/
├── frontend.md
├── backend.md
└── api.md

frontend/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   ├── constants/
│   ├── context/
│   ├── hooks/
│   ├── i18n/
│   ├── pages/
│   ├── routes/
│   ├── schemas/
│   ├── services/
│   ├── types/
│   ├── utils/
│   └── App.tsx

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

## 🖼️ Property Media Strategy

For the graduation version, the easiest approach is:

* Upload property images directly
* Save image URLs in the database
* Store video walkthroughs as embedded YouTube unlisted links or external URLs
* Use basic optimization for gallery images
* Optionally use Cloudinary or S3 later

This avoids:

* Large server storage costs
* Slow media-heavy pages
* Complex video processing
* Expensive hosting

---

## 💡 Business Value

EstateFlow can become a real SaaS platform for:

* Real estate agencies
* Independent brokers
* Property owners
* Rental managers
* Real estate startups
* Property developers

Potential business model:

* Paid property listing submissions
* Featured listing packages
* Commission-based revenue
* Ad placements for agencies or developers
* Premium subscription plans for agencies

---

## 🎓 Graduation Project Presentation

### Problem

Many real estate businesses still use manual methods to manage listings, leads, appointments, and payments.

### Solution

EstateFlow provides a centralized digital platform that simplifies property listing, search, inquiry, and monetization workflows.

### Benefits

* Saves time
* Reduces errors
* Improves listing visibility
* Makes communication easier
* Supports business growth
* Provides analytics and reports

---

## ⚙️ Installation and Setup

### 1. Clone the Repository

```bash id="b9tdfu"
git clone https://github.com/your-username/EstateFlow.git
cd EstateFlow
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
VITE_MAPS_API_KEY=your_maps_api_key
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

The repository currently does not contain the Spring Boot backend folder yet. The steps below describe the planned backend module documented in `docs/backend.md` and `docs/api.md`.

### 6. Navigate to Backend Folder

```bash id="r7t2zc"
cd backend
```

### 7. Configure Database

Create a PostgreSQL database:

```sql id="f0p2wr"
CREATE DATABASE estateFlow_real_estate_db;
```

### 8. Configure Application Properties

Open:

```bash id="g8y4nv"
src/main/resources/application.properties
```

Example configuration:

```properties id="g3h7as"
spring.datasource.url=jdbc:postgresql://localhost:5432/estateFlow_real_estate_db
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
    container_name: estateFlow_real_estate_db
    restart: always
    environment:
      POSTGRES_DB: _real_estate_db
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    ports:
      - "5432:5432"

  backend:
    build: ./backend
    container_name: estateFlow_backend
    ports:
      - "8080:8080"
    depends_on:
      - postgres
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://postgres:5432/estateFlow_real_estate_db
      SPRING_DATASOURCE_USERNAME: postgres
      SPRING_DATASOURCE_PASSWORD: postgres

  frontend:
    build: ./frontend
    container_name:estateFlow_frontend
    ports:
      - "5173:5173"
    depends_on:
      - backend
```

---

## 🧪 Default Development Accounts

```txt id="aw50xq"
Super Admin
Email: admin@estateFlow.com
Password: Admin123!

Agency Admin
Email: agency@estateFlow.com
Password: Agency123!

Agent
Email: agent@estateFlow.com
Password: Agent123!

Property Owner
Email: owner@estateFlow.com
Password: Owner123!

Buyer
Email: buyer@estateFlow.com
Password: Buyer123!

Renter
Email: renter@estateFlow.com
Password: Renter123!
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
