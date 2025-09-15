# **Software Specification: Research Paper Management API**

## **1. Overview**

This is a simple Spring Boot REST API project that manages **Research Papers** with secure access.
Key features:

* **JWT-based authentication** with credentials loaded from a `login.json` file.
* 3 API endpoints.
* **Spring Security** configured correctly 
* Server **hosted** on port `8080`

## **2. Data Model**

### **Entity: ResearchPaper**

| Field       | Type                  | Notes                                 |
| ----------- | --------------------- | ------------------------------------- |
| id          | Long (auto-generated) | Primary Key                           |
| name        | String                | Required                              |
| description | String                | Optional                              |
| date        | LocalDate             | Format: `dd/MM/yyyy`                  |
| status      | Enum                  | Values: `DONE`, `REVIEW`, `SUBMITTED` |
| abstract    | String                | Optional                              |

## **3. Authentication**

* Load username/password from `login.json` file in resources folder.
* The credentials file format:
```json
{
  "user": {
    "username": "admin", 
    "password": "1234"
  }
}
```

## **4. API Endpoints**

### **Login**
* `POST /auth/login`
  * Request: `{ "username": "admin", "password": "1234" }`
  * Response: `{ "token": "<jwt-token>", "expiresIn": 172800 }`
  * JWT token expires after 2 days

### **Research Papers**
(All require JWT Bearer token)

* `POST /api/research-papers`
  * Creates new paper. Auto-generates: `id`, `status` (defaults to `REVIEW`), `date` (current date)
  * Request body:
  ```json
  {
    "name": "test paper",
    "description": "test desc", 
    "abstract": "test abstract"
  }
  ```

* `GET /api/research-papers`
  * Fuzzy search with optional query parameters
  * Searches across all entity fields using partial matching
  * Supports pagination: `?page=0&size=10`
  * Example: `/api/research-papers?name=machine&status=DONE&page=0&size=5`

## **5. Search Requirements**

* Case-insensitive search across **all fields**
* When new fields are added to ResearchPaper entity, they should automatically be searchable
* All parameters optional - no params returns all results with pagination
* Use standard Spring Data pagination format in response

## **6. Technical Requirements**

* Spring Boot with H2 in-memory database
* JWT authentication with proper security configuration
* Date fields must handle European date format (dd/MM/yyyy)
* Application should start with single command: `mvn spring-boot:run`
* No external dependencies beyond standard Spring Boot starters

## **7. Deliverables**

Complete working Spring Boot project with:
- All endpoints functional
- JWT security working
- Search functionality across all fields  
- Proper error handling
- European date format support

**Timeline**: 1 week