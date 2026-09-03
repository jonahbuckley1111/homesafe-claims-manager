# HomeSafe Claims Manager

HomeSafe Claims Manager is a production-minded full-stack web application for managing home inventories, simulated insurance policies, and claims. The project is being developed to practice enterprise application design with Java, Spring Boot, Spring Security, relational database design, and SQL Server.

> **Educational project:** HomeSafe Claims Manager is a simulation. It does not issue real insurance policies, process real claims, or use real customer data.

## Project Status

The application is in early development. Requirements analysis, use-case design, and the initial logical ERD are complete. Work is currently focused on the public-facing HTML and CSS before the Spring Boot backend is introduced.

Completed so far:

- Software requirements specification
- Detailed use cases and acceptance checks
- Logical entity-relationship diagram
- Initial public homepage
- Initial account-registration page
- Shared responsive CSS theme

## Purpose

The project is intended to develop practical experience with:

- Translating business requirements into application behavior
- Structuring a Java and Spring Boot application into clear layers
- Designing normalized relational databases
- Implementing authentication and role-based authorization
- Enforcing business rules on the server
- Managing transactions and immutable history records
- Writing unit, integration, security, and browser-level tests
- Using Git and GitHub throughout the development lifecycle

## User Roles

| Role | Responsibilities |
| --- | --- |
| **User** | Maintains a home inventory, views owned policies, submits claims, and follows public claim status history. |
| **Adjuster** | Reviews assigned claims, adds internal notes, and performs permitted claim-status transitions. |
| **Admin** | Manages accounts, policies, adjusters, claim assignments, privileged actions, and audit information. |

All users authenticate through one login page. The server determines the available pages and actions from the authenticated account's role and its relationship to the requested record.

## Planned Features

### Public and account features

- Informational homepage and project disclaimer
- User registration
- Login and logout
- Role-aware dashboards
- Account activation and deactivation

### Policy features

- Admin creation and maintenance of simulated policies
- Policy activation, cancellation, and historical viewing
- User access limited to policies owned by the authenticated account
- Policy search and filtering

### Home inventory features

- Create, view, edit, and archive inventory items
- Record category, description, purchase information, estimated value, and serial number
- Optionally associate an inventory item with an owned policy
- Preserve inventory records referenced by a claim

### Claims features

- Submit a claim against an eligible active policy
- Generate a unique, human-readable claim number
- Assign or reassign claims to active Adjusters
- Add append-only internal claim notes
- Enforce permitted claim-status transitions
- Record immutable claim-status history
- Present a User-safe public timeline without internal notes

## Claim Lifecycle

The planned claim workflow is:

```text
SUBMITTED
    |
    v
UNDER_REVIEW <----> DOCUMENTS_REQUESTED
    |
    +----> APPROVED ----> PAID ----> CLOSED
    |
    +----> DENIED -----------------> CLOSED
```

Every status change will create a history record in the same database transaction as the claim update.

## Planned Technology Stack

### Backend

- Java
- Spring Boot
- Spring MVC
- Spring Security
- Spring Data JPA
- Hibernate
- Bean Validation
- Maven

### Frontend

- Thymeleaf
- HTML5
- CSS3
- JavaScript
- Bootstrap may be introduced where it improves accessibility and consistency

### Database

- Microsoft SQL Server
- Version-controlled database migrations
- JPA/Hibernate for standard persistence
- Selected JDBC exercises for learning lower-level database access

### Testing

- JUnit 5
- Mockito
- Spring Boot Test
- Integration tests against a controlled test database

## Planned Application Architecture

```text
Browser
   |
   v
Spring MVC Controller
   |
   v
Service Layer
   |
   v
Repository Layer
   |
   v
JPA / Hibernate / JDBC
   |
   v
SQL Server
```

Responsibilities will remain separated:

- **Controllers** handle HTTP requests, validated input, and responses.
- **Services** enforce authorization, business rules, and transaction boundaries.
- **Repositories** perform persistence operations and scoped database queries.
- **Entities** represent persistent domain data.
- **DTOs and form objects** carry validated input and safe output without directly exposing persistence entities.

## Core Data Model

The current logical ERD contains these entities:

- `USER_ACCOUNT`
- `ROLE`
- `ACCOUNT_ROLE`
- `POLICY`
- `CLAIM`
- `CLAIM_ASSIGNMENT`
- `CLAIM_NOTE`
- `CLAIM_STATUS_HISTORY`
- `AUDIT_EVENT`
- `INVENTORY_ITEM`
- `CLAIM_INVENTORY_ITEM`

Important database rules include unique email, policy-number, and claim-number constraints; one current assignment per claim; append-only note and status-history records; and foreign keys protecting all ownership relationships.

## Security Principles

- Passwords will be stored only as strong one-way hashes.
- Public registration will always create a standard User account.
- Only an Admin may create or assign privileged roles.
- Authorization will be enforced by the backend, not only by hidden buttons or links.
- Protected requests will require both a role check and a record-ownership or assignment check.
- Client-provided roles, owners, statuses, totals, and audit fields will not be trusted.
- Credentials, password hashes, session identifiers, and secrets will not be committed to Git or written to application logs.
- Internal claim notes will never be included in User-facing responses.

## Repository Organization

The planned repository structure is:

```text
homesafe-claims-manager/
|-- docs/
|   |-- requirements/
|   |-- use-cases/
|   `-- database/
|-- src/
|   |-- main/
|   |   |-- java/
|   |   `-- resources/
|   |       |-- static/
|   |       |   |-- css/
|   |       |   `-- js/
|   |       `-- templates/
|   `-- test/
|-- .gitignore
|-- README.md
`-- pom.xml
```

The current static prototype may use a simpler file structure until it is moved into the Spring Boot `templates` and `static` directories.

## Getting Started

### Current static prototype

1. Clone the repository using the URL shown under GitHub's **Code** button.
2. Open the repository folder in Visual Studio Code.
3. Open the homepage directly in a browser or use the VS Code Live Server extension.

The forms are currently interface prototypes. Registration, authentication, authorization, and persistence will not function until the backend is implemented.

### Planned Spring Boot application

After the Spring Boot project and Maven Wrapper are added, the application will be started with:

```bash
./mvnw spring-boot:run
```

On Windows PowerShell:

```powershell
.\mvnw.cmd spring-boot:run
```

Required local configuration values will be documented in an `.env.example` or example properties file. Actual credentials and local override files must remain excluded by `.gitignore`.

## Roadmap

- [x] Define application requirements
- [x] Write detailed use cases
- [x] Design the logical ERD
- [x] Create the initial homepage
- [x] Create the initial registration interface
- [ ] Add client-side registration validation
- [ ] Generate the Spring Boot project
- [ ] Create SQL Server migrations
- [ ] Implement account and role entities
- [ ] Implement registration and authentication
- [ ] Implement role-aware dashboards
- [ ] Implement policy management
- [ ] Implement claim submission and assignment
- [ ] Implement claim notes and status history
- [ ] Implement home inventory management
- [ ] Add automated tests
- [ ] Deploy a demonstration environment

## Development Approach

Features will be implemented as small vertical slices. Each slice should include the interface, controller, service logic, persistence, database migration, error handling, and automated tests needed to demonstrate one complete workflow.

The first planned end-to-end slice is:

```text
User registers
    -> User logs in
    -> Admin creates an active policy
    -> User views the policy
    -> User submits a valid claim
    -> Admin assigns an Adjuster
    -> Adjuster reviews the claim
    -> User sees the updated public status
```

## Documentation

Project documentation will be maintained under `docs/`:

- Software Requirements Specification
- Use Case Specification
- Logical ERD
- Data dictionary
- Database migration notes
- Architecture decisions
- Test plan and deployment guide

## License

No open-source license has been selected yet. Unless a license is added, standard copyright protections apply.

## Author

Developed by [Jonah Buckley](https://github.com/jonahbuckley1111) as a personal software-engineering and enterprise application-development project.
