**Project "GoNote"** - Note management system.

**Description:**
Users can create, edit, search, view, and delete notes. Notes can be categorized and prioritized.

### Project Workflow

1. **Authorization Service (NodeJS)**:
    - Registering new users.
    - Authenticating existing users.
    - Issuing JWT tokens for access to the note service.
    - The service can use Postgres to store user data.

2. **Note Service (Golang)**:
    - CRUD operations for notes:
        - Creating notes.
        - Viewing notes.
        - Searching notes based on various criteria, using advanced indexes for fast data retrieval.
        - Modifying and editing notes.
        - Deleting notes.
    - Every user action with notes (creation, viewing, editing, deleting) 
      sends a message to RabbitMQ for further statistics aggregation.

3. **Statistics Aggregation Service (Golang)**:
    - Subscribed to the relevant RabbitMQ queue.
    - Processes events from the note service and aggregates user activity statistics.
    - Stores processed statistics in a separate table or even a separate database.


### Infrastructure:
- **Postgres** - for storing data on users, notes, and statistics.
- **RabbitMQ** - for asynchronous messaging between services.
- **Docker** - each service and infrastructure component (databases, RabbitMQ) is packaged in separate containers.
- **gRPC** - for inter-service communication (retrieving statistics from the aggregation service upon user request).
- **Ansible** - for provisioning and configuring infrastructure components.

### Architectural Patterns and Approaches:
- **DDD (Domain-Driven Design)**: Domain logic and business entities are clearly delineated and separated from infrastructure code.
- **CQRS (Command Query Responsibility Segregation)**: Separating responsibility between commands (state changes) and queries (data retrieval).
- **Hexagonal Architecture (Ports and Adapters)**: The application core (business logic) is separated from external agents like databases, web servers, and other services.
