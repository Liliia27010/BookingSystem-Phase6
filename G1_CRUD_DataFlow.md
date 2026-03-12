# Booking System CRUD

# CREATE – Create Resource

POST /api/resources  
Status: 201 Created

sequenceDiagram
    participant User
    participant Frontend
    participant Backend
    participant PostgreSQL

    User->>Frontend: Fill resource form
    Frontend->>Backend: POST /api/resources
    Backend->>PostgreSQL: INSERT new resource

    alt Success
        PostgreSQL-->>Backend: Resource created
        Backend-->>Frontend: 201 Created
        Frontend-->>User: Show success message
    else Validation error
        Backend-->>Frontend: 400 Bad Request
        Frontend-->>User: Show validation error
    end

# READ – Get Resources

GET /api/resources  
Status: 200 OK

sequenceDiagram
    participant User
    participant Frontend
    participant Backend
    participant PostgreSQL

    User->>Frontend: Open resources page
    Frontend->>Backend: GET /api/resources
    Backend->>PostgreSQL: SELECT * FROM resources

    alt Success
        PostgreSQL-->>Backend: Return resource list
        Backend-->>Frontend: 200 OK
        Frontend-->>User: Display resources
    else Server error
        Backend-->>Frontend: 500 Server Error
        Frontend-->>User: Show error message
    end


# UPDATE – Update Resource

PUT /api/resources/4  
Status: 200 OK

sequenceDiagram
    participant User
    participant Frontend
    participant Backend
    participant PostgreSQL

    User->>Frontend: Edit resource
    Frontend->>Backend: PUT /api/resources/:id
    Backend->>PostgreSQL: UPDATE resource

    alt Success
        PostgreSQL-->>Backend: Resource updated
        Backend-->>Frontend: 200 OK
        Frontend-->>User: Show updated resource
    else Resource not found
        Backend-->>Frontend: 404 Not Found
        Frontend-->>User: Show error message
    end


# DELETE – Delete Resource

DELETE /api/resources/4  
Status: 204 No Content


sequenceDiagram
    participant User
    participant Frontend
    participant Backend
    participant PostgreSQL

    User->>Frontend: Click delete resource
    Frontend->>Backend: DELETE /api/resources/:id
    Backend->>PostgreSQL: DELETE resource

    alt Success
        PostgreSQL-->>Backend: Resource deleted
        Backend-->>Frontend: 204 No Content
        Frontend-->>User: Remove resource from list
    else Resource not found
        Backend-->>Frontend: 404 Not Found
        Frontend-->>User: Show error message
    end
  
