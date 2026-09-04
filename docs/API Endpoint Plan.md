
## Section B - API Endpoint Plan

### Authentication Endpoints

| HTTP Method | Route | Description | Role Required | Request Body | Expected Response |
|-------------|-------|-------------|---------------|--------------|-------------------|
| POST | /api/auth/register | Register a new participant account | None (Public) | { email, password, full_name } | 201 Created - user object with token 400 Bad Request - validation error 409 Conflict - email already exists |
| POST | /api/auth/login | Authenticate user and return JWT token | None (Public) | { email, password } | 200 OK - { token, user } 401 Unauthorized - invalid credentials 404 Not Found - user doesn't exist |
| POST | /api/auth/logout | Invalidate current session token | Any (Logged in) | None | 200 OK - logout successful 401 Unauthorized - no valid token |

