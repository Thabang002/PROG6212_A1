
## Section B - API Endpoint Plan

### Authentication Endpoints

| HTTP Method | Route | Description | Role Required | Request Body | Expected Response |
|-------------|-------|-------------|---------------|--------------|-------------------|
| POST | /api/auth/register | Register a new participant account | None (Public) | { email, password, full_name } | 201 Created - user object with token 400 Bad Request - validation error 409 Conflict - email already exists |
| POST | /api/auth/login | Authenticate user and return JWT token | None (Public) | { email, password } | 200 OK - { token, user } 401 Unauthorized - invalid credentials 404 Not Found - user doesn't exist |
| POST | /api/auth/logout | Invalidate current session token | Any (Logged in) | None | 200 OK - logout successful 401 Unauthorized - no valid token |

### User Profile Endpoints

| HTTP Method | Route | Description | Role Required | Request Body | Expected Response |
|-------------|-------|-------------|---------------|--------------|-------------------|
| GET | /api/users/profile | Get current logged-in user's profile | Any (Logged in) | None | 200 OK - user profile object 401 Unauthorized - no token |
| PUT | /api/users/profile | Update current user's profile | Any (Logged in) | { full_name, email, password? } | 200 OK - updated user profile 400 Bad Request - validation error 409 Conflict - email already exists |
| GET | /api/users/{id}/enrolments | Get all enrolments for a specific user | Organiser or Self | None | 200 OK - list of enrolments 403 Forbidden - not authorized to view 404 Not Found - user doesn't exist |

### Event Endpoints

| HTTP Method | Route | Description | Role Required | Request Body | Expected Response |
|-------------|-------|-------------|---------------|--------------|-------------------|
| GET | /api/events | Get all events with optional filters (date, location, status) | None (Public) | None | 200 OK - list of event objects with pagination |
| GET | /api/events/{id} | Get specific event details with categories and organiser | None (Public) | None | 200 OK - event object 404 Not Found - event doesn't exist |
| POST | /api/events | Create a new event | Organiser | { title, description, date, start_time, location_id, max_participants } | 201 Created - new event object 400 Bad Request - validation error 403 Forbidden - not an organiser |
| PUT | /api/events/{id} | Update an existing event | Organiser (owner only) | { title, description, date, start_time, location_id, max_participants, status } | 200 OK - updated event object 403 Forbidden - not event organiser 404 Not Found - event doesn't exist |
| DELETE | /api/events/{id} | Delete an event (soft delete) | Organiser (owner only) | None | 204 No Content - deletion successful 403 Forbidden - not event organiser 404 Not Found - event doesn't exist |
| GET | /api/events/{id}/categories | Get all categories for a specific event | None (Public) | None | 200 OK - list of category objects 404 Not Found - event doesn't exist |
| GET | /api/events/{id}/enrolments | Get all enrolments for an event (with filters) | Organiser (owner only) | None | 200 OK - list of enrolment objects with participant details 403 Forbidden - not event organiser 404 Not Found - event doesn't exist |
| GET | /api/events/{id}/weather | Get weather forecast for an event | None (Public) | None | 200 OK - weather forecast object 404 Not Found - event or weather doesn't exist |

### Category Endpoints

| HTTP Method | Route | Description | Role Required | Request Body | Expected Response |
|-------------|-------|-------------|---------------|--------------|-------------------|
| GET | /api/categories/{id} | Get specific category details | None (Public) | None | 200 OK - category object 404 Not Found - category doesn't exist |
| POST | /api/events/{id}/categories | Create a new category for an event | Organiser (owner only) | { name, description, distance_km, start_fee, max_participants } | 201 Created - new category object 403 Forbidden - not event organiser 404 Not Found - event doesn't exist |
| PUT | /api/categories/{id} | Update an existing category | Organiser (event owner) | { name, description, distance_km, start_fee, max_participants } | 200 OK - updated category object 403 Forbidden - not event organiser 404 Not Found - category doesn't exist |
| DELETE | /api/categories/{id} | Delete a category | Organiser (event owner) | None | 204 No Content - deletion successful 403 Forbidden - not event organiser 404 Not Found - category doesn't exist |

### Enrolment Endpoints

| HTTP Method | Route | Description | Role Required | Request Body | Expected Response |
|-------------|-------|-------------|---------------|--------------|-------------------|
| POST | /api/events/{id}/enrol | Enrol a participant in an event category | Participant | { category_id, payment_status? } | 201 Created - enrolment object 400 Bad Request - already enrolled or category full 403 Forbidden - not a participant 404 Not Found - event or category doesn't exist |
| GET | /api/enrolments/{id} | Get specific enrolment details | Organiser or Self | None | 200 OK - enrolment object 403 Forbidden - not authorized to view 404 Not Found - enrolment doesn't exist |
| PUT | /api/enrolments/{id} | Update enrolment status (e.g., withdraw) | Participant (self only) | { status } | 200 OK - updated enrolment object 403 Forbidden - not authorized 404 Not Found - enrolment doesn't exist |
| DELETE | /api/enrolments/{id} | Cancel/withdraw an enrolment | Participant (self only) | None | 204 No Content - cancellation successful 403 Forbidden - not authorized 404 Not Found - enrolment doesn't exist |
| PUT | /api/enrolments/{id}/payment | Update payment status of enrolment | Organiser | { payment_status } | 200 OK - updated enrolment 403 Forbidden - not event organiser 404 Not Found - enrolment doesn't exist |

### Result Endpoints

| HTTP Method | Route | Description | Role Required | Request Body | Expected Response |
|-------------|-------|-------------|---------------|--------------|-------------------|
| POST | /api/enrolments/{id}/results | Capture a participant's result | Organiser (event owner) | { finish_time, position, pace_per_km, status, notes } | 201 Created - result object 400 Bad Request - validation error 403 Forbidden - not event organiser 404 Not Found - enrolment doesn't exist |
| GET | /api/results/{id} | Get specific result details | Organiser or Self | None | 200 OK - result object 403 Forbidden - not authorized 404 Not Found - result doesn't exist |
| PUT | /api/results/{id} | Update an existing result | Organiser (event owner) | { finish_time, position, pace_per_km, status, notes } | 200 OK - updated result object 403 Forbidden - not event organiser 404 Not Found - result doesn't exist |
| GET | /api/users/{id}/results | Get all results for a specific participant | Organiser or Self | None | 200 OK - list of result objects with event details 403 Forbidden - not authorized 404 Not Found - user doesn't exist |
| GET | /api/events/{id}/results | Get all results for an event (with ranking) | Organiser (owner only) | None | 200 OK - list of result objects ranked by position 403 Forbidden - not event organiser 404 Not Found - event doesn't exist |
| GET | /api/results/leaderboard | Get leaderboard for a specific event/category | None (Public) | { event_id, category_id? } | 200 OK - ranked list of participants 400 Bad Request - missing required parameters |

### Location Endpoints

| HTTP Method | Route | Description | Role Required | Request Body | Expected Response |
|-------------|-------|-------------|---------------|--------------|-------------------|
| GET | /api/locations | Get all locations | None (Public) | None | 200 OK - list of location objects |
| GET | /api/locations/{id} | Get specific location details | None (Public) | None | 200 OK - location object 404 Not Found - location doesn't exist |
| POST | /api/locations | Create a new location | Organiser | { name, address, city, province, postal_code, latitude, longitude } | 201 Created - location object 400 Bad Request - validation error 403 Forbidden - not an organiser |

