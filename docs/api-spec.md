# KiwiShare RESTful API Specification

This document details the unified RESTful API endpoints exposed by the Koa.js TypeScript backend server inside `server/src/app.ts`.

---

## 1. Authentication Endpoints

### `POST /api/auth/register`
Registers a new user profile with password authentication in MongoDB.
* **Payload**:
  ```json
  {
    "email": "sam@kiwishare.co.nz",
    "password": "securepassword123",
    "displayName": "Sam"
  }
  ```
* **Success Response (201 Created)**:
  ```json
  {
    "status": "success",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "64e1f77bcf86cd799439011",
      "displayName": "Sam",
      "trustScore": 100,
      "isVerified": false
    }
  }
  ```

### `POST /api/auth/login`
Authenticates an existing user and returns a JSON Web Token.
* **Payload**:
  ```json
  {
    "email": "sam@kiwishare.co.nz",
    "password": "securepassword123"
  }
  ```
* **Success Response (200 OK)**:
  ```json
  {
    "status": "success",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "64e1f77bcf86cd799439011",
      "displayName": "Sam",
      "trustScore": 100,
      "isVerified": false
    }
  }
  ```

### `POST /api/auth/send-otp`
Sends a 6-digit one-time password code to the given email address.
* **Payload**:
  ```json
  {
    "email": "sam@kiwishare.co.nz"
  }
  ```
* **Success Response (200 OK)**:
  ```json
  {
    "status": "success",
    "message": "Verification code sent successfully."
  }
  ```

### `POST /api/auth/verify-otp`
Verifies the OTP code and creates or logs in the user profile.
* **Payload**:
  ```json
  {
    "email": "sam@kiwishare.co.nz",
    "code": "123456",
    "displayName": "Sam"
  }
  ```
* **Success Response (200 OK)**:
  ```json
  {
    "status": "success",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "64e1f77bcf86cd799439011",
      "email": "sam@kiwishare.co.nz",
      "displayName": "Sam",
      "avatarUrl": null,
      "trustScore": 100,
      "isVerified": false
    }
  }
  ```

### `POST /api/auth/google`
Validates Google OAuth ID tokens and signs in the user.
* **Payload**:
  ```json
  {
    "idToken": "google_id_token_string"
  }
  ```
* **Success Response (200 OK)**:
  ```json
  {
    "status": "success",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "64e1f77bcf86cd799439011",
      "email": "sam@kiwishare.co.nz",
      "displayName": "Sam",
      "avatarUrl": "https://...",
      "trustScore": 100,
      "isVerified": true
    }
  }
  ```

---

## 2. Used Items Resource (`/api/usedItems`)

### `GET /api/usedItems`
Queries and filters available used items from the marketplace.
* **Query Parameters**:
  - `category` (optional): `Plants`, `Camping`, `Furniture`, `Transport`, `clothing`, `electronics`, `vehicle`
  - `query` (optional): Search string matched against title, category, description, and location.
  - `status` (optional): `active`, `reserved`, `sold`
  - `sellerId` / `ownerId` (optional): Filter items by specific seller ID.
* **Success Response (200 OK)**:
  ```json
  [
    {
      "id": "64e1f77bcf86cd799439012",
      "title": "Retro Armchair",
      "priceNzd": "45",
      "location": "Central, Auckland",
      "imageUrl": "https://images.unsplash.com/photo-1567538096630-e0c55bd6374c",
      "isSustainable": true,
      "category": "Furniture",
      "condition": "good",
      "status": "active",
      "ownerId": "64e1f77bcf86cd799439011"
    }
  ]
  ```

### `GET /api/usedItems/:id`
Retrieves full details for a single used item by its ID.
* **Path Parameter**:
  - `id`: MongoDB ObjectId or unique item ID.
* **Success Response (200 OK)**:
  ```json
  {
    "id": "64e1f77bcf86cd799439012",
    "title": "Retro Armchair",
    "description": "Comfortable vintage armchair in great shape.",
    "priceNzd": "45",
    "price": 4500,
    "currency": "NZD",
    "location": "Central, Auckland",
    "imageUrl": "https://images.unsplash.com/photo-1567538096630-e0c55bd6374c",
    "isSustainable": true,
    "category": "Furniture",
    "condition": "good",
    "status": "active",
    "ownerId": "64e1f77bcf86cd799439011",
    "createdAt": "2026-08-19T00:00:00.000Z"
  }
  ```
* **Error Response (404 Not Found)**:
  ```json
  {
    "status": "error",
    "message": "Used item not found."
  }
  ```

### `POST /api/usedItems`
Publishes a new used item listing to the marketplace (Requires Authorization Header).
* **Headers**:
  - `Authorization: Bearer <JWT_TOKEN>`
* **Payload**:
  ```json
  {
    "title": "Monstera Deliciosa",
    "priceNzd": "15",
    "location": "Te Aro, Wellington",
    "imageUrl": "https://images.unsplash.com/photo-1545241047-6083a3684587",
    "isSustainable": true,
    "category": "Plants",
    "condition": "like_new",
    "description": "Healthy indoor plant in ceramic pot."
  }
  ```
* **Success Response (201 Created)**:
  ```json
  {
    "status": "created",
    "item": {
      "id": "64e1f77bcf86cd799439099",
      "title": "Monstera Deliciosa",
      "priceNzd": "15",
      "location": "Te Aro, Wellington",
      "imageUrl": "https://images.unsplash.com/photo-1545241047-6083a3684587",
      "isSustainable": true,
      "category": "Plants",
      "status": "active",
      "ownerId": "64e1f77bcf86cd799439011"
    }
  }
  ```

### `PATCH /api/usedItems/:id` (or `PUT /api/usedItems/:id`)
Updates an existing used item listing (Requires Auth & Ownership).
* **Headers**:
  - `Authorization: Bearer <JWT_TOKEN>`
* **Payload**:
  ```json
  {
    "priceNzd": "12",
    "status": "reserved"
  }
  ```
* **Success Response (200 OK)**:
  ```json
  {
    "status": "success",
    "item": {
      "id": "64e1f77bcf86cd799439099",
      "title": "Monstera Deliciosa",
      "priceNzd": "12",
      "status": "reserved"
    }
  }
  ```

### `DELETE /api/usedItems/:id`
Deletes/soft-deletes a used item listing (Requires Auth & Ownership).
* **Headers**:
  - `Authorization: Bearer <JWT_TOKEN>`
* **Success Response (200 OK)**:
  ```json
  {
    "status": "success",
    "message": "Used item deleted successfully."
  }
  ```

> **Backwards Compatibility**: `/api/listings` endpoints remain supported as aliases to `/api/usedItems`.

---

## 3. User Profile Resource (`/api/users`)

### `GET /api/users/me`
Retrieves profile data of the currently authenticated user.
* **Headers**:
  - `Authorization: Bearer <JWT_TOKEN>`
* **Success Response (200 OK)**:
  ```json
  {
    "status": "success",
    "user": {
      "id": "64e1f77bcf86cd799439011",
      "email": "sam@kiwishare.co.nz",
      "displayName": "Sam",
      "avatarUrl": "https://...",
      "trustScore": 100,
      "isVerified": true
    }
  }
  ```

### `PATCH /api/users/me`
Updates profile information of the current user.
* **Headers**:
  - `Authorization: Bearer <JWT_TOKEN>`
* **Payload**:
  ```json
  {
    "displayName": "Sam Yao",
    "avatarUrl": "https://images.unsplash.com/..."
  }
  ```
* **Success Response (200 OK)**:
  ```json
  {
    "status": "success",
    "user": {
      "id": "64e1f77bcf86cd799439011",
      "email": "sam@kiwishare.co.nz",
      "displayName": "Sam Yao",
      "avatarUrl": "https://images.unsplash.com/...",
      "trustScore": 100,
      "isVerified": true
    }
  }
  ```

### `GET /api/users/me/usedItems` (or `/api/users/me/listings`)
Retrieves all items listed by the authenticated user.
* **Headers**:
  - `Authorization: Bearer <JWT_TOKEN>`
* **Query Parameters**:
  - `status` (optional): `sold` or `active,reserved`
* **Success Response (200 OK)**:
  ```json
  [
    {
      "id": "64e1f77bcf86cd799439099",
      "title": "Monstera Deliciosa",
      "priceNzd": "15",
      "status": "active"
    }
  ]
  ```

---

## 4. Transaction & QR Handover Endpoints

### `POST /api/transactions/handover/claim`
Executes atomic ownership status transitions during peer-to-peer item handovers.
* **Headers**:
  - `Authorization: Bearer <JWT_TOKEN>`
* **Payload**:
  ```json
  {
    "itemId": "64e1f77bcf86cd799439099",
    "claimCode": "QR_HANDOVER_TOKEN_ABC"
  }
  ```
* **Success Response (200 OK)**:
  ```json
  {
    "status": "success",
    "message": "Ownership transaction verified and committed successfully.",
    "newOwnerId": "64e1f77bcf86cd799439011"
  }
  ```
