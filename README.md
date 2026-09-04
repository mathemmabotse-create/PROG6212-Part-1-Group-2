1. Project Overview

RaceDay is an event management system designed to manage running and road-race events.

The system allows participants to:

* Register and log in.
* Manage their profiles.
* View available events.
* View race categories.
* Enrol in events.
* View their results.

Organisers can:

* Create and manage events.
* Add categories to events.
* Manage participant enrolments.
* Record race results.

⸻

2. Project Objectives

The main objectives of the RaceDay system are to:

* Allow users to register and log in securely.
* Allow users to view and manage their profiles.
* Allow organisers to create, update and manage race events.
* Allow participants to view available events and categories.
* Allow participants to enrol in race events.
* Prevent duplicate enrolments.
* Allow organisers to record race results.
* Maintain relationships between users, events, categories, enrolments and results.
* Store information in a structured SQL Server database.

⸻

3. User Roles

Organiser

An organiser can:

* Log in.
* Create events.
* View their events.
* Update their own events.
* Delete their own events where permitted.
* Manage event categories.
* Manage participant enrolments.
* Record race results.

An organiser can only modify their own events.

The API must check that:

Logged-in UserID = Event OrganiserID

This prevents one organiser from editing another organiser’s event.

Participant

A participant can:

* Register an account.
* Log in.
* View and update their profile.
* View available events.
* View event categories.
* Enrol in events.
* View their enrolments.
* View their race results.

Public User

Users who are not logged in can access public information such as available events and categories, depending on the endpoint.

⸻

4. Database Structure

The RaceDay system uses Microsoft SQL Server.

The main tables are:

USERS

Stores information about users, including organisers and participants.

EVENTS

Stores information about running events.

Examples include:

* Event name
* Description
* Event date
* Location
* Capacity
* Status
* Organiser

CATEGORIES

Stores race categories such as:

* 5 KM
* 10 KM
* 21 KM
* 42 KM

EVENTCATEGORIES

Connects events with categories.

This table is used because one event can have multiple categories, and a category can be used for multiple events.

ENROLMENTS

Stores information about participants who enrol in events.

RESULTS

Stores the results achieved by participants.

⸻

5. Database Relationships

The main relationships are:

USERS
   |
   | Organiser
   ↓
EVENTS
   |
   ↓
EVENTCATEGORIES
   |
   ↓
CATEGORIES
USERS
   |
   ↓
ENROLMENTS
   |       |
   |       ↓
   |     EVENTS
   |
   ↓
CATEGORIES
   |
   ↓
RESULTS

The EVENTCATEGORIES table resolves the many-to-many relationship between EVENTS and CATEGORIES.

⸻

6. API Endpoint Plan

The RaceDay API is divided into several main areas.

Authentication

The authentication endpoints allow users to:

* Register an account.
* Log in.

User Profile

Profile endpoints allow logged-in users to:

* View their profile.
* Update their profile.

Events

Event endpoints allow users to:

* View all events.
* View a specific event.
* Create an event.
* Update an event.
* Delete an event where permitted.

Categories

Category endpoints allow users to:

* View categories.
* Create categories where permitted.
* Update categories where permitted.
* Delete categories where permitted.

Event Categories

These endpoints allow the system to:

* View categories belonging to an event.
* Add a category to an event.
* Remove a category from an event.

Event Enrolments

These endpoints allow participants to:

* Enrol in an event.
* View their enrolments.
* View enrolments.
* Cancel an enrolment where permitted.

Results

These endpoints allow the system to:

* View results.
* View a participant’s result.
* Record results.
* Update results where permitted.

The implemented API in Part 2 should closely match the API Endpoint Plan.

⸻

7. API Rules

Authentication

Protected endpoints require the user to be logged in.

Role-Based Access

The API checks the user’s role before allowing restricted operations.

For example:

ORGANISER → Can create/manage events
PARTICIPANT → Can enrol in events

Owner Check

For event update and delete operations, being an organiser is not enough.

The API must also check that the organiser owns the event.

Logged-in UserID
       =
EVENTS.OrganiserID

This prevents one organiser from changing another organiser’s event.

Duplicate Enrolments

A participant should not be allowed to create a duplicate enrolment.

A successful enrolment returns:

201 Created

If the enrolment conflicts with an existing enrolment:

409 Conflict

⸻

8. Common HTTP Responses

200 OK

The request was successful.

201 Created

A new record was successfully created.

400 Bad Request

The information supplied by the user is invalid.

401 Unauthorized

The user needs to log in or has provided invalid authentication.

403 Forbidden

The user is logged in but does not have permission to perform the action.

404 Not Found

The requested record does not exist.

409 Conflict

The request conflicts with existing information, such as a duplicate enrolment.

⸻

9. SQL Database Script

The SQL database script is stored inside the /docs folder.

The script:

* Creates the RaceDay database.
* Creates all required tables.
* Creates primary keys.
* Creates foreign keys.
* Creates NOT NULL constraints.
* Creates UNIQUE constraints.
* Creates DEFAULT values.
* Creates CHECK constraints.
* Inserts sample users.
* Inserts sample events.
* Inserts sample categories.
* Links events and categories.
* Inserts sample enrolments.
* Inserts sample results.

The script can be executed using SQL Server Management Studio (SSMS).

⸻

10. Sample Data

The database contains sample data for testing.

Users

The database contains:

* 2 organisers
* 2 participants

Events

The sample events are:

1. Johannesburg City Run
2. Pretoria Spring Marathon
3. Limpopo Fun Run

Categories

The sample categories are:

1. 5 KM
2. 10 KM
3. 21 KM
4. 42 KM

The categories are connected to events through the EVENTCATEGORIES table.

⸻

11. Project Folder Structure

The project can be organised as follows:

RaceDay/
│
├── docs/
│   ├── README.md
│   ├── api-endpoint-plan.md
│   ├── database.sql
│   └── erd.pdf
│
├── src/
│   └── ...
│
└── ...

The /docs folder contains the project’s documentation, ERD, API plan and SQL database script.

⸻

12. Technologies Used

The RaceDay system uses:

* C# / .NET — API development
* Microsoft SQL Server — database
* SQL Server Management Studio (SSMS) — database development and testing
* REST API — communication between the application and database
* Git/GitHub — version control and project submission

⸻

13. Setup Instructions

Step 1: Create the Database

Open SQL Server Management Studio.

Connect to the SQL Server instance.

Open:

/docs/database.sql

Run the script.

Check that these tables have been created:

USERS
EVENTS
CATEGORIES
EVENTCATEGORIES
ENROLMENTS
RESULTS

Step 2: Run the API

Open the API project in the development environment.

Update the database connection string so that it connects to the RACEDAY database.

Build and run the API.

Step 3: Test the API

Test the endpoints using the API testing tool being used for the project.

Test authentication first, followed by:

1. User profile
2. Events
3. Categories
4. Event categories
5. Enrolments
6. Results

⸻

14. Documentation

The RaceDay project contains the following documentation:

ERD

Shows the database entities, attributes and relationships.

API Endpoint Plan

Shows:

* HTTP method
* Route
* Description
* Role required
* Request body
* Expected response

SQL Database Script

Creates and populates the SQL Server database.

README

Explains the project, database structure, API functionality and setup instructions.

⸻

15. Conclusion

RaceDay provides a structured system for managing running events, participants, race categories, enrolments and results.

The database, ERD and API Endpoint Plan are designed to work together. The API implementation in Part 2 should follow the planned endpoints and enforce the appropriate authentication, role and ownership rules.
