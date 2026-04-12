# 🚖 Ride Booking Platform

A robust and scalable backend for a Ride Booking Application, built with **Spring Boot** and **Java 21**. This project demonstrates a real-world implementation of a ride-hailing service, featuring location-based routing, dynamic pricing strategies, driver matching algorithms, and a secure wallet payment system.

## 🌟 Features

### 👤 User Roles & Authentication
- **Secure JWT Authentication**: Stateless authentication using JSON Web Tokens (JWT).
- **Role-Based Access Control (RBAC)**: Distinct roles for **RIDER**, **DRIVER**, and **ADMIN**.
- **Onboarding Workflow**: Separate signup flows for Riders and Drivers (Admin approval supported).

### 🚗 Ride Management
- **Ride Request**: Riders can request rides with pickup/drop-off locations.
- **Driver Matching**: Smart algorithms to find the nearest available driver.
- **Ride Lifecycle**: 
  - `REQUESTED` -> `BOOKED` -> `STARTED` -> `ENDED`
- **OTP Verification**: Secure ride start using a One-Time Password (OTP) system.
- **Ride Cancellation**: Riders or Drivers can cancel pending rides.

### 💰 Wallet & Payments
- **Internal Wallet System**: Each user has a dedicated wallet for transactions.
- **Transaction History**: Complete audit log of all credits and debits (`WalletTransaction`).
- **Payment Methods**: Extensible design supporting secure wallet transfers (easily expandable to Stripe/PayPal).

### ⭐ Rating System
- **Two-Way Reviews**: Riders rate Drivers, and Drivers rate Riders.
- **Automated Rating Calculation**: Integration of ratings into user profiles.

### 📍 Location & Routing
- **Geospatial Data**: Efficient handling of coordinates using **PostGIS** and **Hibernate Spatial**.
- **Distance & Time Calculation**: Integration with **OSRM (Open Source Routing Machine)** for accurate route data.

## 🛠️ Technology Stack

| Component | Technology |
|-----------|------------|
| **Language** | Java 21 |
| **Framework** | Spring Boot 3.3.1 |
| **Database** | PostgreSQL (with PostGIS extension) |
| **ORM** | Hibernate (Spring Data JPA) |
| **Security** | Spring Security, JWT |
| **Mapping** | ModelMapper |
| **Documentation** | SpringDoc OpenAPI (Swagger UI) |
| **Tools** | Lombok, Maven |

## 🏗️ Architecture & Design Patterns

This project follows a clean **Layered Architecture** and utilizes industry-standard design patterns to ensure maintainability and scalability.

### Project Structure
```
src/main/java/com/tusaryan/project/uber/uberApp
├── configs         # App configurations (Beans, Security, OpenAPI)
├── controllers     # REST API Controllers
├── dtos            # Data Transfer Objects
├── entities        # JPA Entities (Database Schema)
├── exceptions      # Custom global exception handling
├── repositories    # Spring Data repositories
├── services        # Business Logic Interface & Impl
├── strategies      # Strategy Pattern Implementations
└── security        # JWT & Auth Filters
```

### Key Design Patterns
- **Strategy Pattern**: Used to decouple algorithms from the business logic, making the system flexible.
  - `DriverMatchingStrategy`: Switch between different matching logics (e.g., `DriverMatchingHighestRatedStrategy`, `DriverMatchingNearestStrategy`).
  - `RideFareCalculationStrategy`: Dynamically calculate fares (e.g., `RideFareDefaultFareCalculationStrategy`, `RideFareSurgePricingFareCalculationStrategy`).
  - `PaymentStrategy`: Handle various payment methods (`WalletPaymentStrategy`, `CashPaymentStrategy`).
- **Facade Pattern**: Simplified service interfaces (`RideService`, `AuthService`) exposing complex underlying logic.

## 💾 Database Schema

The core entities driving the application:
- **User**: Base entity for all users (email, password, roles).
- **Rider**: Profile for customers (+ rating).
- **Driver**: Profile for drivers (+ rating, vehicle status, current location).
- **Ride**: Represents a trip (pickup, drop-off, status, fare, OTP).
- **Wallet**: Users' financial balance.
- **Payment**: Records of payment attempts and statuses.

## 🚀 Setup & Installation Guide

### Prerequisites
- **Java 21 SDK**
- **PostgreSQL Database** (with PostGIS extension enabled)
- **Maven**

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/ride-booking-app.git
cd ride-booking-app
```

### 2. Configure Database
Ensure PostgreSQL is running and create a database named `uber-postgres`.
Update `src/main/resources/application.properties`:
```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/uber-postgres
spring.datasource.username=your_postgres_username
spring.datasource.password=your_postgres_password
```

### 3. SMTP & Security Configuration
Configure your mail server (for OTP/Notifications) and JWT Secret in `application.properties`:
```properties
jwt.secretKey=YOUR_SECURE_LONG_SECRET_KEY
spring.mail.username=your_email@gmail.com
spring.mail.password=your_app_password
```

### 4. Build and Run
```bash
mvn clean install
mvn spring-boot:run
```

The application will start on `http://localhost:8080`.

## 📚 API Documentation

Once the application is running, explore the interactive API documentation via Swagger UI:

👉 **[Swagger UI URL](http://localhost:8080/swagger-ui.html)**

### Key Endpoints
- **Auth**: `/auth/signup`, `/auth/login`, `/auth/refresh`
- **Riders**: `/riders/requestRide`, `/riders/rateDriver`
- **Drivers**: `/drivers/acceptRide`, `/drivers/startRide`, `/drivers/endRide`

---
*Developed by [Aryan]*
