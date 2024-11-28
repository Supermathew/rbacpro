# rbacpro



```markdown
# Django REST API with JWT Authentication, Throttling, and Refresh Token Management

## Project Overview

This project implements a robust Django REST API for user authentication and management using JWT (JSON Web Tokens). It provides features like user registration, login, token generation, token refresh, and revocation of old tokens. Throttling mechanisms are also in place to prevent abuse of API endpoints.

### Features
- **User Registration**: Users can register with unique email IDs.
- **Login with JWT**: Generates access and refresh tokens upon login. Old tokens are invalidated to enhance security.
- **Token Refresh**: Enables users to refresh their access tokens without logging in again.
- **Throttling**: Limits the number of API calls to prevent abuse.
- **Role Management**: Users can have roles like Admin, Moderator, or User.
- **Secure Authentication**: Uses Django REST Framework and Simple JWT for secure token-based authentication.

---

## Requirements

Ensure the following are installed on your system:
- Python 3.8+
- Django 4.x+
- Django REST Framework
- djangorestframework-simplejwt

---

## Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/your-repository.git
   cd your-repository
   ```

2. **Create a virtual environment**:
   ```bash
   python -m venv venv
   source venv/bin/activate  # For Linux/Mac
   venv\Scripts\activate     # For Windows
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Apply migrations**:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

5. **Run the development server**:
   ```bash
   python manage.py runserver
   ```

---

## Usage

### 1. Register a User
**Endpoint**: `POST /api/register/`  
**Payload**:
```json
{
  "username": "user1",
  "email": "user1@example.com",
  "password": "strongpassword",
  "role": "user"
}
```
**Response** (Success):
```json
{
  "id": 1,
  "username": "user1",
  "email": "user1@example.com",
  "role": "user"
}
```

### 2. Login
**Endpoint**: `POST /api/login/`  
**Payload**:
```json
{
  "email": "user1@example.com",
  "password": "strongpassword"
}
```
**Response** (Success):
```json
{
  "access": "ACCESS_TOKEN_HERE",
  "refresh": "REFRESH_TOKEN_HERE"
}
```

### 3. Refresh Access Token
**Endpoint**: `POST /api/token/refresh/`  
**Payload**:
```json
{
  "refresh": "REFRESH_TOKEN_HERE"
}
```
**Response** (Success):
```json
{
  "access": "NEW_ACCESS_TOKEN_HERE"
}
```

This endpoint allows you to refresh your access token using the refresh token. The refresh token has a longer validity period than the access token and can be used until it expires or is revoked.

---

### 4. Access a Protected Resource
**Endpoint**: `GET /api/protected/`  
**Headers**:
```http
Authorization: Bearer ACCESS_TOKEN_HERE
```

**Response** (Success):
```json
{
  "message": "You have accessed a protected resource!"
}
```

This route/resources can only be accessed by admin or moderator not by user.

If the access token is expired, use the `/api/token/refresh/` endpoint to generate a new access token.

---

## Throttling

### Global Throttling Policy
- **Authenticated Users**: 20 requests per minute
- **Anonymous Users**: 30 requests per minute

If the limit is exceeded, the response will be:
```json
{
  "detail": "Request was throttled. Expected available in X seconds."
}
```

---

## Running Tests

To ensure the application is working as expected, run the following tests:

1. **Test Cases**:

### User Guide for Testing

#### **Test Case 1: User Registration**
- **Action**: Send a `POST` request to `/api/register/`.
- **Expected**: User is created successfully with a unique email.

#### **Test Case 2: Login and Token Generation**
- **Action**: Send a `POST` request to `/api/login/`.
- **Expected**: Access and refresh tokens are generated.

#### **Test Case 3: Token Refresh**
- **Action**: Send a `POST` request to `/api/token/refresh/` using a valid refresh token.
- **Expected**: A new access token is generated.

#### **Test Case 4: Revocation of Old Tokens**
- **Action**: Login twice with the same credentials.
- **Expected**: Old tokens are invalidated, and only the latest tokens are valid.

#### **Test Case 5: Throttling for Authenticated Users**
- **Action**: Make more than 5 requests per minute to a protected route.
- **Expected**: A `429 Too Many Requests` response is returned.

#### **Test Case 6: Protected Route**
- **Action**: Access a protected route with a valid token.
- **Expected**: Route is successfully accessed.

---

## Additional Notes

- Use tools like [Postman](https://www.postman.com/) or `curl` to test the API.
- Ensure you replace `"ACCESS_TOKEN_HERE"` and `"REFRESH_TOKEN_HERE"` with valid tokens in requests.

---

## Author
**Your Name**  
Feel free to reach out at [dolikemathewalex@gmail.com](mailto:dolikemathewalex@gmail.com) for queries or suggestions.
```

---

### What's New:
1. **Refresh Token Details**:
   - Included how to use `/api/token/refresh/` to regenerate access tokens.
   - Explained the role of refresh tokens in extending user sessions securely.

2. **Additional Test Case**:
   - Added **Test Case 3: Token Refresh** for validating refresh token usage.

This ensures users can easily understand and test the refresh token functionality along with the rest of the system.
