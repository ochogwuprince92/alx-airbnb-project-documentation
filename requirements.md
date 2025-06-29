# 📄 Airbnb Clone – Backend Requirement Specifications

## 🎯 Objective
This document defines the **technical** and **functional** requirements for three core backend features of the Airbnb Clone: **User Authentication**, **Property Management**, and **Booking System**. These specifications include endpoint definitions, input/output formats, validation rules, and performance expectations.

## 🔐 1. User Authentication

### 📌 Functional Requirements
- Allow users to register and log in as a guest or host.
- Secure login using JWT-based session handling.
- Validate all inputs and prevent duplicate emails.

### 🧩 Endpoints

#### `POST /api/auth/register/`
- **Description**: Register a new user.
- **Input**:
  ```json
  {
    "email": "user@example.com",
    "password": "securePassword123",
    "role": "guest"  // or "host"
  }
Validations:

Email must be unique and valid.

Password must be at least 8 characters.

Role must be one of: guest, host.

Output:

{
  "message": "User registered successfully",
  "token": "<JWT_TOKEN>",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "role": "guest"
  }
}
POST /api/auth/login/
Description: Authenticate user and issue JWT token.

Input:

{
  "email": "user@example.com",
  "password": "securePassword123"
}
Output:

{
  "token": "<JWT_TOKEN>",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "role": "guest"
  }
}
⚙️ Performance Criteria
Token generation and response must occur within 500ms under load.

Authentication errors should return within 200ms.

🏡 2. Property Management
📌 Functional Requirements
Hosts can create, update, and delete listings.

Listings include fields like title, description, price, availability, images, and amenities.

🧩 Endpoints
POST /api/properties/
Description: Create a new property listing.

Input:

{
  "title": "2-Bedroom Apartment in Lagos",
  "description": "A comfortable space near the beach.",
  "location": "Lagos, Nigeria",
  "price_per_night": 150,
  "amenities": ["WiFi", "Air Conditioning"],
  "availability": {
    "start_date": "2025-07-01",
    "end_date": "2025-12-31"
  }
}
Validations:

Price must be a positive number.

Dates must be future and valid range.

Output:

{
  "message": "Property created successfully",
  "property_id": 101
}
PUT /api/properties/{id}/
Description: Update property details.

DELETE /api/properties/{id}/
Description: Delete a listing (host or admin only).

⚙️ Performance Criteria
Listing creation must occur under 1 second.

Database writes should be optimized with indexing on location, host_id.

📆 3. Booking System
📌 Functional Requirements
Guests can book properties by specifying check-in/check-out dates.

The system must prevent double bookings.

Bookings should have status tracking: pending, confirmed, cancelled, completed.

🧩 Endpoints
POST /api/bookings/
Description: Book a property for selected dates.

Input:

{
  "property_id": 101,
  "check_in": "2025-08-01",
  "check_out": "2025-08-05"
}
Validations:

Dates must be within the property’s availability.

No existing bookings must conflict with selected dates.

Output:

{
  "message": "Booking request submitted",
  "booking_id": 55,
  "status": "pending"
}
GET /api/bookings/{id}/
View booking details.

DELETE /api/bookings/{id}/
Cancel booking (guest or host based on role).

⚙️ Performance Criteria
Double booking check must complete within 200ms using indexed queries.

Bookings should be confirmed within 1 second after validation.

🧾 Summary
Feature	Key Endpoints	Input Validation	Response Time
User Authentication	/api/auth/register/, /login/	Email, password	≤ 500ms
Property Management	/api/properties/, /edit/	Price, dates	≤ 1s
Booking System	/api/bookings/	Date conflict	≤ 1s