# Unified API Specification

This document details the unified API endpoints exposed by the Koa.js TypeScript gateway inside `functions/src/app.ts`.

---

## 1. Authentication Endpoints

### `POST /api/auth/register`
Creates a new user profile document in Firestore.
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
      "id": "user_uid_123",
      "displayName": "Sam",
      "trustScore": 100,
      "isVerified": false
    }
  }
  ```

### `POST /api/auth/login`
Authenticates a user and returns a JSON Web Token.
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
      "id": "user_uid_123",
      "displayName": "Sam",
      "trustScore": 100,
      "isVerified": false
    }
  }
  ```

---

## 2. Listing Endpoints

### `GET /api/listings`
Fetches a list of sustainable products, optionally filtered by category.
* **Query Parameters**:
  - `category` (optional): `Plants`, `Camping`, `Furniture`, `Transport`
* **Success Response (200 OK)**:
  ```json
  [
    {
      "id": "item_uuid_99",
      "title": "Retro Armchair",
      "priceNzd": "45",
      "location": "Auckland",
      "imageUrl": "https://...",
      "isSustainable": true,
      "category": "Furniture",
      "status": "active"
    }
  ]
  ```

### `POST /api/listings`
Publishes a new listing to the marketplace (Requires Authorization Header).
* **Headers**:
  - `Authorization: Bearer <JWT_TOKEN>`
* **Payload**:
  ```json
  {
    "title": "Monstera Deliciosa",
    "priceNzd": "15",
    "location": "Wellington",
    "imageUrl": "https://...",
    "isSustainable": true,
    "category": "Plants"
  }
  ```
* **Success Response (201 Created)**:
  ```json
  {
    "status": "created",
    "item": {
      "id": "item_uuid_new",
      "title": "Monstera Deliciosa",
      "priceNzd": "15",
      "location": "Wellington",
      "imageUrl": "https://...",
      "isSustainable": true,
      "category": "Plants",
      "status": "active",
      "ownerId": "user_uid_123"
    }
  }
  ```

### `GET /api/listings/:id`
Fetches one visible listing and the public seller context required by the item detail page.

* **Success Response (200 OK)**:
  ```json
  {
    "id": "item_uuid_99",
    "title": "Retro Armchair",
    "description": "A comfortable pre-loved armchair.",
    "condition": "good",
    "priceNzd": "45",
    "location": "Central, Auckland",
    "imageUrl": "https://...",
    "imageUrls": ["https://..."],
    "isSustainable": true,
    "category": "Furniture",
    "status": "active",
    "ownerId": "user_uid_123",
    "sellerName": "Sam",
    "sellerAvatarUrl": "https://...",
    "sellerRating": 4.8,
    "sellerReviewCount": 12
  }
  ```
* **Not Found (404)**: Returned for an unknown, hidden, draft, or deleted listing.

### `PUT /api/listings/:id`
Updates an active or reserved listing. The authenticated user must own the listing.

* **Headers**:
  - `Authorization: Bearer <JWT_TOKEN>`
* **Payload**:
  ```json
  {
    "title": "Retro Armchair",
    "description": "A comfortable pre-loved armchair.",
    "condition": "good",
    "category": "Furniture",
    "priceNzd": "45",
    "location": "Central, Auckland",
    "negotiable": true,
    "isSustainable": true
  }
  ```
* **Success Response (200 OK)**: `{ "status": "updated", "item": { ... } }`
* **Validation Error (400)**: The editable listing fields are invalid.
* **Unauthorized/Forbidden (401/403)**: Missing token or the caller does not own the listing.
* **Not Found (404)**: The listing is unknown or is not editable in its current state.

---

## 3. Transaction & QR Handover Endpoints

### `POST /api/transactions/handover/claim`
Executes atomic status transitions for sustainable products during peer-to-peer exchanges (Requires Auth).
* **Headers**:
  - `Authorization: Bearer <JWT_TOKEN>`
* **Payload**:
  ```json
  {
    "itemId": "item_uuid_new",
    "ownerId": "owner_uid_456",
    "claimCode": "QR_HANDOVER_TOKEN_ABC"
  }
  ```
* **Success Response (200 OK)**:
  ```json
  {
    "status": "success",
    "message": "Ownership transaction verified and committed successfully.",
    "newOwnerId": "user_uid_123"
  }
  ```
