
# RaceDay - Part 1: System Planning and Database

## Project Overview
RaceDay is a full-stack event management system for South African road running, walking, and cycling events. This repository contains the system planning and database design for Part 1 of the project.

##  Repository Structure
```
RaceDay/
├── docs/
│   ├── erd.png                    # Entity Relationship Diagram
│   ├── api-endpoints.md           # API Endpoint Plan
│   └── raceday-schema.sql         # SQL Database Script
└── README.md
```

##  Features
- **Two User Roles**: Organisers and Participants with role-based access
- **Event Management**: Create, update, and delete events
- **Category Management**: Define event categories with distances and fees
- **Participant Enrolment**: Register for events with category selection
- **Results Tracking**: Capture and view participant performance
- **Weather Information**: Race day weather forecasts

##  Database Schema
The database consists of **8 entities**:
- **Users** - System users with role differentiation
- **Locations** - Event venues and addresses
- **Events** - Core event management
- **EventOrganisers** - Junction table for event organisers
- **Categories** - Event categories (5km, 10km, etc.)
- **Enrolments** - Participant registrations
- **Results** - Participant performance results
- **WeatherData** - Race day weather information

### ERD Preview
```
Users ──< EventOrganisers >── Events ──┬── Categories
  │                                     ├── Enrolments ── Results
  │                                     ├── Locations
  │                                     └── WeatherData
  │
  └─────────────────────────────────────┘
```

##  Technology Stack
- **Database**: Microsoft SQL Server (SSMS)
- **Script Language**: T-SQL
- **Diagram Tool**: Draw.io / Lucidchart

##  Key Constraints
- Unique email addresses for users
- Check constraints for role types (organiser/participant)
- Foreign key relationships maintain data integrity
- Cascade delete on dependent tables where appropriate
- Unique constraints prevent duplicate enrolments

##  Sample Data
The database seeds with:
- **2 Organisers**: Thabo Mokoena, Zanele Ndlovu
- **2 Participants**: Sipho Dlamini, Linda Ngcobo
- **3 Events**: Comrades, Two Oceans, Soweto Marathons
- **Multiple categories** per event (3 each)
- **Sample enrolments** with various statuses
- **Sample results** for completed events

## Getting Started

### Prerequisites
- SQL Server Management Studio (SSMS)
- SQL Server (2019 or later)

### Installation
1. Clone the repository:
```bash
git clone https://github.com/yourusername/raceday.git
cd raceday
```

2. Open SSMS and execute the SQL script:
```sql
-- Connect to your SQL Server instance
-- Open and execute: docs/raceday-schema.sql
```

3. Verify the database creation:
```sql
USE RaceDayDB;
SELECT * FROM Users;
SELECT * FROM Events;
```

##  API Endpoints (Planned)
The system will expose **35+ endpoints** across:
- **Authentication**: Register, Login, Logout
- **Users**: Profile management
- **Events**: CRUD operations with filtering
- **Categories**: Manage event categories
- **Enrolments**: Register and manage participation
- **Results**: Capture and view performance data
- **Locations**: Venue management
- **Weather**: Race day forecasts

##  Verification Queries
Run these to verify your database setup:

```sql
-- Check all tables
SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES;

-- View sample data
SELECT * FROM Users;
SELECT * FROM Events;
SELECT * FROM Enrolments WITH (NOLOCK);
```

##  Notes
- All passwords in seed data use placeholders (`$2a$12$HASHEDPASSWORD#`)
- Use strong hashing (bcrypt/Argon2) in production
- Latitude/Longitude support for location mapping
- Time fields support race start times and finish times
- Decimal types for distance, pace, and fees

##  Related Documentation
- **ERD**: Detailed entity relationships in `docs/erd.png`
- **API Plan**: Complete endpoint specifications in `docs/api-endpoints.md`
- **Database**: Full schema with constraints in `docs/raceday-schema.sql`

##  Security Considerations
- Password hashing required before storage
- Role-based access control at API level (Part 2)
- Input validation for all user inputs
- SQL injection protection (use parameterized queries)

##  Contact
For questions or clarifications:
- Project Lead: [Your Name]
- Email: [your.email@institution.edu]

---
