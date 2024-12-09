Here is the documentation converted to the **Markdown** format:

API Documentation
=================

Response Types
--------------

### User

| Field | Type | Notes |
| --- | --- | --- |
| `name` | `object` |  |
| `first` | `string` |  |
| `middle` | `string` |  |
| `last` | `string` |  |
| `phone` | `string` | Is a valid Israeli phone number |
| `email` | `string` | Unique key |
| `password` | `string` | Trimmed, required, and unique key |
| `image` | `object` | Includes a unique key |
| `url` | `string` |  |
| `alt` | `string` |  |
| `address` | `string` | Unique key |
| `isAdmin` | `boolean` | Defaults to `false` |
| `isBusiness` | `boolean` | Defaults to `false` |
| `createdAt` | `string` | Automatically generated on creation |
| `lockUntil` | `Date` | Default value `null` |
| `failedLoginAttempts` | `Number` | Default value `0` |

* * * * *

### Post

| Field | Type | Notes |
| --- | --- | --- |
| `title` | `object` | Uses default validation |
| `description` | `string` | Max length is 1024 characters |
| `image` | `object` |  |
| `url` | `string` | URL of the image |
| `alt` | `string` | Alt text for the image |
| `bizNumber` | `number` | Required, between 1000000 and 9999999 |
| `likes` | `array` | Array of strings (user IDs who liked the post) |
| `comments` | `array` | Array of strings (user comments) |
| `createdAt` | `date` | Automatically generated at creation |
| `user_id` | `objectId` | References the user who created the post, required |

* * * * *

Users API
---------

**Base endpoint:** `/users`

### 1\. **GET All Users**

-   **Endpoint:** `/users`
-   **Method:** `GET`
-   **Authentication:** Required (Admin only)
-   **Response:**
    -   Returns an array of User objects if the requester has Admin privileges.
    -   Returns a `403 Forbidden` status if the requester is not an Admin.

### 2\. **POST New User**

-   **Endpoint:** `/users`
-   **Method:** `POST`
-   **Request Body:**
    -   Fields: `name`, `phone`, `email`, `password`, `image`, `address`, `isAdmin`, `isBusiness`.
-   **Response:**
    -   Returns the created user object along with the user ID.

#### Example Request Body:

```json
{
  "name": {
    "first": "BUSINESS",
    "middle": "USER",
    "last": "FOR DEMONSTRATION"
  },
  "phone": "050-1234567",
  "email": "BUSINESS@gmail.com",
  "password": "securePassword123",
  "image": {
    "url": "https://via.placeholder.com/150",
    "alt": "Profile Image"
  },
  "address": {
    "state": "Tel Aviv",
    "country": "Israel",
    "city": "Tel Aviv",
    "street": "Shinkin",
    "houseNumber": "3",
    "zip": "12345"
  },
  "isAdmin": false,
  "isBusiness": true
}
```

### 3\. **GET User by ID**

-   **Endpoint:** `/users/:id`
-   **Method:** `GET`
-   **Authentication:** Required (Admin only)
-   **Response:**
    -   Returns a single user object corresponding to the specified `id`.

### 4\. **POST Login**

-   **Endpoint:** `/users/login`
-   **Method:** `POST`
-   **Request Body:**
    -   Fields: `email`, `password`.
-   **Response:**
    -   Returns a token if the login is successful.
    -   Returns an error if the login is unsuccessful.

#### Example Request Body:

```json
{
  "email": "BUSINESS@gmail.com",
  "password": "securePassword123"
}
```


### 5\. **PUT Edit User**

-   **Endpoint:** `/users/:id`
-   **Method:** `PUT`
-   **Authentication:** Required (User's own info or Admin only)
-   **Request Body:**
    -   Fields: Any editable fields in `User`.
-   **Response:**
    -   Returns the updated user object.

#### Example Request Body:

```json

{
  "name": {
    "first": "Linoy",
    "middle": "Pe'er",
    "last": "Example"
  },
  "phone": "050-1234567",
  "email": "linoy.peer@example.com",
  "password": "$2a$10$S",
  "image": {
    "url": "https://via.placeholder.com/150",
    "alt": "profile image"
  },
  "address": {
    "state": "Tel Aviv",
    "country": "Israel",
    "city": "Tel Aviv",
    "street": "Shinkin",
    "houseNumber": 3,
    "zip": 12345
  }
}
```


### 6\. **PATCH Change User Status**

-   **Endpoint:** `/users/:id/status`
-   **Method:** `PATCH`
-   **Authentication:** Required (Admin only)
-   **Request Body:**
    -   Fields: `isBusiness`.
-   **Response:**
    -   Returns the updated user status (business or guest).

### 7\. **DELETE User**

-   **Endpoint:** `/users/:id`
-   **Method:** `DELETE`
-   **Authentication:** Required (Admin only)
-   **Response:**
    -   Returns a confirmation message for the deleted user.

* * * * *

Cards API
---------

**Base endpoint:** `/cards`

### 1\. **POST Create Card**

-   **Endpoint:** `/cards`
-   **Method:** `POST`
-   **Authentication:** Required (Business users or Admin only)
-   **Request Body:**
    -   Fields: `title`, `description`, `image`, `bizNumber`, `likes`, `comments`.
-   **Response:**
    -   Returns the created card object.

#### Example Request Body:

```json
{
  "title": "Create New Card Garden Flowers",
  "description": "Spring is here and the flowers are blooming everywhere! Loving the vibrant colors and fresh smells of nature.",
  "image": {
    "url": "https://images.app.goo.gl/HymjtKTWAMRbjqmw8",
    "alt": "Flower garden"
  },
  "user_id": "67262ddbe519deea9c89c8e0"
}
```

### 2\. **GET All Cards**

-   **Endpoint:** `/cards`
-   **Method:** `GET`
-   **Response:**
    -   Returns an array of Card objects.

### 3\. **GET User's Cards**

-   **Endpoint:** `/cards/my-cards`
-   **Method:** `GET`
-   **Authentication:** Required (User's posts or Admin only)
-   **Response:**
    -   Returns an array of cards created by the authenticated user.

### 4\. **GET Card by ID**

-   **Endpoint:** `/cards/:id`
-   **Method:** `GET`
-   **Response:**
    -   Returns a single card object corresponding to the specified `id`.

### 5\. **PUT Update Card**

-   **Endpoint:** `/cards/:id`
-   **Method:** `PUT`
-   **Authentication:** Required (User's card or Admin only)
-   **Request Body:**
    -   Fields: `title`, `description`, `image`, `bizNumber`.
-   **Response:**
    -   Returns the updated card object.

#### Example Request Body:

```json


`{
  "title": "Delightful Garden Flowers",
  "description": "Spring is here and the flowers are blooming everywhere! Loving the vibrant colors and fresh smells of nature.",
  "image": {
    "url": "https://images.app.goo.gl/HymjtKTWAMRbjqmw8",
    "alt": "Flower garden"
  },
  "user_id": "67262ddbe519deea9c89c8e0"
}`

### 6\. **PATCH Like Card**

-   **Endpoint:** `/cards/:id`
-   **Method:** `PATCH`
-   **Authentication:** Required (Business users only)
-   **Response:**
    -   Adds/removes a like by the authenticated user.
    -   Returns the card object with updated likes.

### 7\. **DELETE Card**

-   **Endpoint:** `/cards/:id`
-   **Method:** `DELETE`
-   **Authentication:** Required (User's card or Admin only)
-   **Response:**
    -   Returns a confirmation message for the deleted card.

### 8\. **PATCH Update Card's Biz Number**

-   **Endpoint:** `/cards/:id`
-   **Method:** `PATCH`
-   **Authentication:** Required (Business users only)
-   **Request Body:**
    -   `bizNumber`: 7-digit number (between 1000000 and 9999999)
-   **Response:**
    -   Returns the updated card object with the new `bizNumber`.

#### Example Request Body:

```json
{
  "newBizNumber": 4584122
}
```


### 9\. **Add Comment to a Card**

-   **Endpoint:** `/cards/:id/comments`
-   **Authentication:** Required (Business users or Admin only)
-   **Request Body:**
    -   `comment`: string (The comment text to be added)
-   **Response:**
    -   Returns an array of comments.

#### Example Request Body:

```json
{
  "comment": "This is a new comment"
}
```


#### Responses:

-   **201 Created**: Returns the updated card object with the new comment.
-   **400 Bad Request**: If there's an issue with the request, such as a missing or invalid comment or card ID.
-   **404 Not Found**: If the card with the specified ID does not exist.

* * * * *

Additional Notes
----------------

-   **Status Codes:** Each endpoint will return the appropriate status code (200 for confirmations, 201 for creating new records, 400 for general request errors, 403 for unauthorized access, and 404 when the requested record is not found).
-   **Authentication:** Actions that require permissions will be secured with JWT for authorized users, and actions that require additional permissions, such as Admin status, will require further validation from the server.

* * * * *

### Headers:

plaintext


`x-auth-token : token of logged user`