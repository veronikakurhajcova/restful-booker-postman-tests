# Restful Booker API – Postman Test Suite

This repository contains a fully structured and functional test suite for the [Restful Booker API](https://restful-booker.herokuapp.com), created using **Postman**.

## Features

- **Positive tests** for all HTTP methods (GET, POST, PUT, DELETE)
- **Negative tests** (e.g., unauthorized access, invalid input)
- **Security tests** (e.g., SQL injection, ID manipulation)
- **Test chaining** with environment variable usage
- **End-to-End Flow** from token creation to resource deletion

---

## Run Tests via Newman

**Prerequisites:**  
- [Node.js](https://nodejs.org/)  
- [Newman](https://www.npmjs.com/package/newman)

### Installation

```bash
npm install -g newman
```

```newman run collections/restful-booker-tests.postman_collection.json \
    --environment environments/restful-booker-env.postman_environment.json \
    --reporters cli,html \
    --reporter-html-export report.html
```

## Test Coverage

### 1. Positive Tests
- `GET` all bookings
- `GET` booking by ID
- `POST` create booking
- `PUT` update booking
- `DELETE` remove booking

### 2. Negative Tests
- Non-existing IDs
- Empty or malformed request bodies
- Invalid login credentials

### 3. Security Tests
- SQL injection attempt
- Endpoint abuse attempts

### 4. End-to-End Flow
- Generate token
- Create booking
- Get booking details
- Update booking (e.g., change firstname)
- Delete booking

## Known Issues & Weaknesses (Summary)

1. **Missing type validation**  
   - The API accepts strings instead of numbers (e.g., `totalprice` can be `"100"` or `"1 OR 1=1"`) without errors.  
   - This risks data inconsistency and potential exploitation.

2. **Required fields accept empty values**  
   - `firstname` can be an empty string `""`, which is meaningless.  
   - The API does not validate minimum length.

3. **Incorrect HTTP status codes**  
   - The API often returns `500 Internal Server Error` instead of `400 Bad Request` for invalid inputs.

4. **Lack of format and range checks**  
   - No length checks on names, dates, negative prices, etc.  
   - Unnatural dates like check-in after check-out are accepted.

5. **Potential for XSS attacks**  
   - The API allows storing and returning raw `<script>` tags in fields like `additionalneeds`, posing a security risk.

---

### Recommendations

- Implement strict type and format validation for inputs.  
- Enforce minimum and maximum lengths for text fields.  
- Fix the API to return proper `400 Bad Request` for invalid data.  
- Implement input/output escaping and sanitization on the backend.  
- Maintain a list of known issues to support developers and test reports.

---

Addressing these weaknesses will significantly improve API quality and security.

 




