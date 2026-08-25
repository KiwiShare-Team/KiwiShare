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
  - `category` (optional): Exact category value returned by `GET /api/usedItems/discovery-options`.
  - `query` (optional): Search string matched against title, category, description, and location.
  - `location` (optional): Exact city value returned by the discovery-options endpoint.
  - `minPrice` / `maxPrice` (optional): Inclusive NZD price bounds.
  - `sustainable` (optional): Set to `true` to include only sustainability-labelled items.
  - `sort` (optional): `price_asc` or `price_desc`; omit for recommended order.
  - `latitude` / `longitude` (optional, paired): Approximate current location for nearby discovery.
  - `radiusKm` (optional): Nearby radius in kilometres; defaults to 50 when coordinates are supplied.
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
      "ownerId": "64e1f77bcf86cd799439011",
      "latitude": -36.8485,
      "longitude": 174.7633
    }
  ]
  ```

### `GET /api/usedItems/discovery-options`
Returns categories, conditions, city locations, approximate city centres, item
counts, and available price bounds derived from MongoDB. Categories come from
the active `Category` collection; condition values are aggregated from stored
listings. The mobile client does not maintain hard-coded marketplace options.

* **Success Response (200 OK)**:
  ```json
  {
    "categories": [
      { "value": "Furniture", "count": 8 }
    ],
    "locations": [
      {
        "value": "Auckland",
        "count": 12,
        "latitude": -36.8485,
        "longitude": 174.7633
      }
    ],
    "conditions": [
      { "value": "refurbished", "count": 3 }
    ],
    "priceRange": {
      "minimum": 15,
      "maximum": 890
    }
  }
  ```

### `GET /api/usedItems/:id`
Retrieves full details for a single used item by its ID.
* **Path Parameter**:
  - `id`: MongoDB ObjectId or unique item ID.
* **Success Response (200 OK)**:
  ```json
  {
    "status": "success",
    "item": {
      "id": "64e1f77bcf86cd799439012",
      "title": "Retro Armchair",
      "description": "Comfortable vintage armchair in great shape.",
      "priceNzd": "45",
      "price": 4500,
      "currency": "NZD",
      "location": "Central, Auckland",
      "imageUrl": "https://example.com/items/armchair.jpg",
      "isSustainable": true,
      "negotiable": true,
      "category": "Furniture",
      "condition": "refurbished",
      "status": "active",
      "ownerId": "64e1f77bcf86cd799439011",
      "seller": {
        "id": "64e1f77bcf86cd799439011",
        "displayName": "Stored seller name",
        "rating": 4.8,
        "reviewCount": 12,
        "trustScore": 87
      },
      "createdAt": "2026-08-19T00:00:00.000Z"
    }
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
Updates an existing used item listing (Requires Auth & Ownership). The detail
editor sends only listing-owned editable fields; owner IDs, moderation status,
view counts, and audit fields cannot be overwritten by the client.
* **Headers**:
  - `Authorization: Bearer <JWT_TOKEN>`
* **Payload**:
  ```json
  {
    "title": "Restored Armchair",
    "description": "Updated description",
    "priceNzd": "12",
    "category": "Furniture",
    "condition": "refurbished",
    "location": "Central, Auckland",
    "negotiable": true,
    "isSustainable": true
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
      "condition": "refurbished"
    }
  }
  ```

### `DELETE /api/usedItems/:id`
Soft-deletes a used item listing (Requires Auth & Ownership). The item remains
stored with `status: deleted` and `deletedAt`; related transaction, report,
review, and audit records are retained.
* **Headers**:
  - `Authorization: Bearer <JWT_TOKEN>`
* **Success Response (200 OK)**:
  ```json
  {
    "status": "success",
    "message": "Used item deleted successfully."
  }
  ```

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
