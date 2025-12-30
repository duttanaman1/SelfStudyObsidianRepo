# Best Practices when writing a REST API?
#### **1. Use Consistent Resource Naming**

- Use **nouns** for resources, not verbs:
	`GET /users POST /orders`
- Pluralize resource names:
	`GET /users/123`
- Use **hierarchical relationships**:
	`GET /users/123/orders`
---
#### **2. HTTP Methods Should Match Intent**

| Method | Purpose                 |
| ------ | ----------------------- |
| GET    | Retrieve resource(s)    |
| POST   | Create resource         |
| PUT    | Update/replace resource |
| PATCH  | Partial update          |
| DELETE | Remove resource         |

---
### **3. Use Proper HTTP Status Codes**

- **2xx** → Success
	- `200 OK`, `201 Created`, `204 No Content`
- **4xx** → Client errors
	- `400 Bad Request`, `401 Unauthorized`, `404 Not Found`
- **5xx** → Server errors
	- `500 Internal Server Error`, `503 Service Unavailable`
---
### **4. Accept and Return JSON**

- Use `application/json` for request and response.
- Example:
	`{   "id": 123,   "name": "Naman Dutta",   "email": "naman@example.com" }`
---
### **5. Use Query Parameters for Filtering, Sorting, and Pagination**

`GET /users?age=25&sort=name&limit=10&page=2`

- Helps avoid huge payloads and improves performance.
---
### **6. Version Your API**

- Include version in the URL:
	`/api/v1/users`
- Makes it easier to release breaking changes without affecting existing clients.   

---
### **7. Provide Clear Error Messages**

- Include **status code, message, and optional error details**:
	`{   "status": 404,   "error": "User not found",   "details": "No user exists with id 123" }`
---
### **8. Security Best Practices**

- Use **HTTPS** for all endpoints.
- Authenticate using **OAuth2, JWT, or API keys**.
- Validate and sanitize input to prevent injection attacks.
- Rate limit APIs to prevent abuse.
---
### **9. Keep APIs Stateless**

- Each request should contain all necessary info.
- Avoid server-side session storage.
---
### **10. Documentation & Discoverability**

- Use **OpenAPI/Swagger** for clear documentation.
- Include examples, required fields, and possible errors.
- Consider providing an **API playground**.
---

### **11. Optimize Performance**

- Implement **caching** with HTTP headers (`ETag`, `Cache-Control`).
- Avoid N+1 queries in backend.
- Use pagination and limit data returned.
---

### **12. Follow HATEOAS (Optional, for Hypermedia APIs)**

- Include links to related resources for easy navigation:
	
	`{   "id": 123,   "name": "Naman",   "_links": {     "self": "/users/123",     "orders": "/users/123/orders"   } }`

# Benefits of using a lambda expressions
### **1. Concise and Readable Code**

- Replaces bulky anonymous inner classes with compact expressions.
- Less boilerplate → easier to read and maintain.
### **2. Enables Functional Programming**

- Treat **functions as first-class citizens**.
- Pass behavior (functions) directly to methods:
### **3. Improves Collection Processing**

- Works well with **Streams API** for filtering, mapping, reducing:
- Simplifies operations like sorting, filtering, aggregating.
### **4. Encourages Reusable and Composable Code**

- Lambdas can be **assigned to variables** or **passed as arguments**.
### **5. Reduces Boilerplate Code**

- Eliminates the need for implementing interfaces like `Runnable` or `Comparator` repeatedly.
### **6. Can Lead to Better Performance**

- Sometimes **enables optimizations** via JVM (like invokedynamic).
- Useful in parallel streams:
### **7. Encourages Immutability and Side-Effect-Free Programming**

- Easier to write **pure functions** when using lambdas, improving **thread safety**.

# Functional Interface, Predicate vs Consumer

# Optional Class in Java 8

# Why not a good practice to return Optional in setters and getters

# What static methods resolved
# Map vs FlatMap: Usecase