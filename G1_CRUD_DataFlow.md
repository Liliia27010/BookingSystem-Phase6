# G1 – CRUD Data Flow: Booking System (Phase 6)

---

# 1️⃣ CREATE – Resource (Sequence Diagram)

```mermaid
sequenceDiagram
    participant U as User (Browser)
    participant F as Frontend (form.js and resources.js)
    participant B as Backend (Express Route)
    participant S as Resource Service
    participant V as express-validator
    participant DB as PostgreSQL

    U->>F: Submit form
    F->>F: Client-side validation
    F->>B: POST /api/resources (JSON)
    B->>S: createResource(data)

    S->>V: Validate request
    V-->>S: Validation result

    alt Validation fails (express-validator)
        S-->>B: Errors in validation
        B-->>F: 400 Bad Request
        F-->>U: Show validation error message
    else Validation OK
        S->>DB: INSERT INTO resources
        DB-->>S: Result

        alt Success
            S-->>B: Created resource
            B-->>F: 201 Created
            F-->>U: Show success message
        else Duplicate
            S-->>B: Duplicate detected
            B-->>F: 409 Conflict
            F-->>U: Show duplicate message
        else Database error
            S-->>B: Database error
            B-->>F: 500 Internal Server Error
            F-->>U: Show database error
        end
    end
```

---

# 2️⃣ READ – Resource (Sequence Diagram)

## READ – All Resources

```mermaid
sequenceDiagram
    participant U as User (Browser)
    participant F as Frontend (form.js and resources.js)
    participant B as Backend (Express Route)
    participant S as Resource Service
    participant DB as PostgreSQL

    U->>F: Request page
    F->>B: GET /api/resources (JSON)
    B->>S: selectResources()
    S->>DB: SELECT FROM resources
    DB-->>S: Result

    alt Success
        S-->>B: Selected resources
        B-->>F: 200 OK
        F-->>U: Render resource list (JSON)
    else Database error
        S-->>B: Database error
        B-->>F: 500 Internal Server Error
        F-->>U: Show database error
    end
```

## READ – Single Resource by ID

```mermaid
sequenceDiagram
    participant U as User (Browser)
    participant F as Frontend (form.js and resources.js)
    participant B as Backend (Express Route)
    participant S as Resource Service
    participant DB as PostgreSQL

    U->>F: Request resource detail
    F->>B: GET /api/resources/:id (JSON)
    B->>S: selectResource(id)

    alt Invalid ID
        S-->>B: Invalid ID
        B-->>F: 400 Bad Request
        F-->>U: Show invalid ID message
    else Valid ID
        S->>DB: SELECT FROM resources WHERE id = :id
        DB-->>S: Result

        alt Success
            S-->>B: Selected resource
            B-->>F: 200 OK
            F-->>U: Render resource detail
        else Not found
            S-->>B: Resource not found
            B-->>F: 404 Not Found
            F-->>U: Show not found message
        else Database error
            S-->>B: Database error
            B-->>F: 500 Internal Server Error
            F-->>U: Show database error
        end
    end
```

---

# 3️⃣ UPDATE – Resource (Sequence Diagram)

```mermaid
sequenceDiagram
    participant U as User (Browser)
    participant F as Frontend (form.js and resources.js)
    participant B as Backend (Express Route)
    participant S as Resource Service
    participant V as express-validator
    participant DB as PostgreSQL

    U->>F: Submit edit form
    F->>F: Client-side validation
    F->>B: PUT /api/resources/:id (JSON)
    B->>S: updateResource(id, data)

    S->>V: Validate request
    V-->>S: Validation result

    alt Invalid ID
        S-->>B: Invalid ID
        B-->>F: 400 Bad Request
        F-->>U: Show invalid ID message
    else Validation fails (express-validator)
        S-->>B: Errors in validation
        B-->>F: 400 Bad Request
        F-->>U: Show validation error message
    else Validation OK
        S->>DB: UPDATE resources SET ... WHERE id = :id
        DB-->>S: Result

        alt Success
            S-->>B: Updated resource
            B-->>F: 200 OK
            F-->>U: Show success message
        else Not found
            S-->>B: Resource not found
            B-->>F: 404 Not Found
            F-->>U: Show not found message
        else Duplicate
            S-->>B: Duplicate detected
            B-->>F: 409 Conflict
            F-->>U: Show duplicate message
        else Database error
            S-->>B: Database error
            B-->>F: 500 Internal Server Error
            F-->>U: Show database error
        end
    end
```

---

# 4️⃣ DELETE – Resource (Sequence Diagram)

```mermaid
sequenceDiagram
    participant U as User (Browser)
    participant F as Frontend (form.js and resources.js)
    participant B as Backend (Express Route)
    participant S as Resource Service
    participant DB as PostgreSQL

    U->>F: Click delete button
    F->>B: DELETE /api/resources/:id
    B->>S: deleteResource(id)

    alt Invalid ID
        S-->>B: Invalid ID
        B-->>F: 400 Bad Request
        F-->>U: Show invalid ID message
    else Valid ID
        S->>DB: DELETE FROM resources WHERE id = :id
        DB-->>S: Result

        alt Success
            S-->>B: Deleted resource
            B-->>F: 204 No Content
            F-->>U: Show success message
        else Not found
            S-->>B: Resource not found
            B-->>F: 404 Not Found
            F-->>U: Show not found message
        else Database error
            S-->>B: Database error
            B-->>F: 500 Internal Server Error
            F-->>U: Show database error
        end
    end
```