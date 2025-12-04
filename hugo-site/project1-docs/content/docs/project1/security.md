---
title: "Security"
weight: 3
---

## Security Features of REST APIs

## Security Mechanisms Implemented

My REST API implements multiple layers of security to protect user data, authentication tokens, and authorization logic.

The main mechanisms include JWT-based authentication, role-based access control, password hashing, token revocation, input validation, and error handling.

Each of these features ensures both user privacy and system integrity.

## 1. JSON Web Token (JWT) Authentication

- Purpose:
  - JWTs are used to verify that a client is authenticated before accessing protected endpoints
  - After successful login, the server issues a JWT signed using a secret key. This token is sent in the Authorization header for subsequent requests
- Implementation Details:
  - A valid token is required in the format:

  ```php
     Authorization: Bearer <token>
  ```

- The token contains:
  - iss (issuer) → identifies the system (project1)
  - iat (issued at) → timestamp when issued
  - exp (expires at) → token expiration time (1 hour)
  - user_id → identifies the authenticated user

### Protected Endpoints

| Endpoint | Method | Description |
|-------------|------|-------------|
| /api/users/me/password | POST | Only accessible to logged-in users |
| /api/users/:id | GET | Requires authentication |
| /api/friendRequests | GET, POST | Requires a valid user token |
| /api/friendRequests/:id | PUT, DELETE | Requires token verification |

### Protected Endpoints: (continued)

| Endpoint | Method | Description |
|-------------|------|-------------|
| /api/friends | GET, DELETE | Requires token verification |
| /api/auth/logout | POST | Requires valid token to revoke session |
| /api/users | GET | (admin-only) Requires valid admin tokenc|

**Files Used:**
/api/middleware.php → handles JWT verification and decoding.

/api/routes/auth.php → issues JWT during login.

## 2. Password Hashing and Verification

- Purpose:
  - To protect stored passwords from exposure even if the database is compromised.
  - Plaintext passwords are never stored in the database.
- Implementation Details:
  - Passwords are hashed using PHP’s built-in password_hash() with the PASSWORD_BCRYPT algorithm.
  - Verification uses password_verify() during login.

### Endpoints Using This Mechanism

| Endpoint | Method | Description |
|-------------|------|-------------|
| /api/users/register | POST | Hashes the password before saving |
| /api/auth/login | POST | Verifies hash match before issuing token |
| /api/users/me/password | POST | (admin-only) Hashes new password before updating |

**Benefits**
Prevents rainbow table attacks.
Protects against insider data exposure.

## 3. Role-Based Access Control (RBAC)

- Purpose:
  - To restrict certain endpoints to only users with administrative privileges.
- Implementation Details:
  - Every user record includes a role field ('user' or 'admin').
  - The middleware function requireAdmin() validates the JWT, retrieves the user, and checks if role === 'admin'.
  - Non-admins receive a 403 Forbidden response.

### Admin-Only Endpoints

| Endpoint | Method | Description |
|-------------|------|-------------|
| /api/users | GET | Admins can view all registered users |
| /api/users/:id | DELETE | Admins can delete user accounts |
| /api/admin/users/:id | GET | Admins can inspect specific user profiles |

**Files Used:**
/api/middleware.php → requireAdmin() function.

**Benefit:**
Enforces the principle of least privilege, preventing normal users from accessing sensitive data or performing system-level actions.

## 4. Token Revocation (Logout Mechanism)

- Purpose:
  - To ensure tokens can be invalidated before their expiration time, supporting secure logouts
- Implementation Details:
  - When a user logs out, their JWT is stored in the revokedTokens table.
  - The middleware checks the table before validating tokens.
  - If a token is found in revokedTokens, access is denied.

### Endpoints Using This Mechanism

| Endpoint | Method | Description |
|-------------|------|-------------|
| /api/auth/logout | POST | Adds token to revocation list |
| (Middleware check) | - | Blocks requests with revoked tokens |

**Database Table:**

- revokedTokens(id, token, revokedAt)
  
**Benefit:**

- Prevents session reuse after logout and provides a clear audit trail of revoked sessions.

---

## 5. Input Validation and Sanitization

- Purpose:
  - To prevent SQL injection, malformed input, and malicious payloads.
- Implementation Details:
  - All SQL queries use prepared statements with bound parameters.
  - Input data is parsed using json_decode() and checked for required fields before queries.
  - Validation ensures proper username, password length, and friend request parameters.

### Endpoints Using This Mechanism

| Endpoint | Method | Description |
|-------------|------|-------------|
| /api/users/register | POST | Validates required username/password |
| /api/auth/login | POST | Validates credentials |
| /api/friendRequests | POST | Validates IDs before insert |
| /api/users/me/password | POST | Validates password before update |

**Example Code Snippet:**

```php
$stmt = $pdo->prepare("SELECT id FROM users WHERE username = ?");
$stmt->execute([$username]);
```

**Benefit:**

- Eliminates direct SQL injection vulnerabilities.

## 6. Error Handling and Response Security

- Purpose:
  - To prevent data leaks and ensure consistent, secure responses.
- Implementation Details:
  - Sensitive errors (like SQL exceptions) are caught and hidden from users.
  - All responses use JSON format for clarity and safety.
  - HTTP status codes (401, 403, 404) are correctly set for each scenario.
  - Authorization errors do not reveal internal logic.

### Endpoints Using This Mechanism

- Applies to all routes across /api/auth/*, /api/users/*, /api/friendRequests, and /api/friends.

**Example:**

```php
http_response_code(401);
echo json_encode(["error" => "Unauthorized"]);
```

**Benefit:**

- Reduces information exposure that could aid attackers.

## 7. CORS and Content-Type Enforcement

- Purpose:
  - To ensure only valid requests with proper headers are processed.
- Implementation Details:
  - Enforces Content-Type: application/json for POST requests.
  - Can be extended with CORS headers (Access-Control-Allow-Origin) for cross-domain safety.

### Endpoints Using This Mechanism

- Applies globally to all POST endpoints:
  - /api/users/register
  - /api/auth/login
  - /api/friendRequests
  - /api/users/me/password
  - /api/auth/logout
