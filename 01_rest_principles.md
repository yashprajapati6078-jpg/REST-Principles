# REST Principles: Resources, Verbs, Idempotency, and Statelessness

## Introduction

REST (Representational State Transfer) is a popular architectural style used for designing web APIs. It provides a standard way for clients and servers to communicate using HTTP. Understanding REST principles is important because they help developers create scalable, reliable, and easy-to-maintain applications. The core concepts of REST include resources, endpoints, HTTP verbs, statelessness, and idempotency.

## Resources vs Endpoints

A **resource** is any piece of data or object that an API manages. Resources are usually represented as nouns and describe the entities within a system.

Examples of resources:

* `/users` – Represents user accounts.
* `/orders` – Represents customer orders.
* `/invoices` – Represents billing invoices.

An **endpoint** is the specific URL through which a client interacts with a resource.

### Resource and Endpoint Examples

| Resource | Endpoint         | Description                   |
| -------- | ---------------- | ----------------------------- |
| Users    | `/users`         | Retrieve all users            |
| Users    | `/users/101`     | Retrieve user with ID 101     |
| Orders   | `/orders`        | Retrieve all orders           |
| Orders   | `/orders/25`     | Retrieve order with ID 25     |
| Invoices | `/invoices`      | Retrieve all invoices         |
| Invoices | `/invoices/5001` | Retrieve invoice with ID 5001 |

In simple terms, a resource is the object being managed, while an endpoint is the address used to access that object.

## HTTP Verbs and Their Semantics

REST APIs use HTTP verbs to define the action that should be performed on a resource.

### GET

GET is used to retrieve data from the server without modifying it.

Example:

```http
GET /users/101
```

This request fetches information about user 101.

### POST

POST is used to create a new resource.

Example:

```http
POST /users
```

Request Body:

```json
{
  "name": "Yash"
}
```

This request creates a new user.

### PUT

PUT is used to replace or completely update an existing resource.

Example:

```http
PUT /users/101
```

Request Body:

```json
{
  "name": "Yash Prajapati",
  "email": "yash@example.com"
}
```

This replaces the existing user data with the new information.

### PATCH

PATCH is used to update specific fields of an existing resource.

Example:

```http
PATCH /users/101
```

Request Body:

```json
{
  "email": "newemail@example.com"
}
```

This updates only the email field while leaving other data unchanged.

### DELETE

DELETE is used to remove a resource.

Example:

```http
DELETE /users/101
```

This removes user 101 from the system.

## HTTP Verb Reference Table

| Verb   | Safe? | Idempotent? | Typical Use                  |
| ------ | ----- | ----------- | ---------------------------- |
| GET    | Yes   | Yes         | Retrieve data                |
| POST   | No    | No          | Create a new resource        |
| PUT    | No    | Yes         | Replace an existing resource |
| PATCH  | No    | No          | Partially update a resource  |
| DELETE | No    | Yes         | Delete a resource            |

A request is considered **safe** if it only reads data and does not modify anything on the server. A request is **idempotent** if repeating it multiple times produces the same final result as executing it once.

## Statelessness

Statelessness is one of the most important REST principles. A stateless server does not store client session information between requests. Every request must contain all the information needed to process it.

For example:

```http
GET /orders
Authorization: Bearer abc123xyz
```

The authentication token is sent with the request. If another request is made later, the token must be sent again. The server does not rely on memory of previous requests.

Benefits of statelessness include:

* Improved scalability
* Better reliability
* Easier load balancing
* Simpler server design

Because every request is independent, any server can handle any request without requiring stored session data.

## Idempotency in Practice

Idempotency ensures that repeating the same request does not produce unexpected results. This is especially useful when network failures cause clients to retry requests.

### Example: PUT Request (Idempotent)

Suppose an order status needs to be updated.

```http
PUT /orders/25
```

Request Body:

```json
{
  "status": "Delivered"
}
```

If the request is sent once:

```text
Order 25 → Delivered
```

If the same request is accidentally sent three more times:

```text
Order 25 → Delivered
Order 25 → Delivered
Order 25 → Delivered
```

The final state remains the same. Therefore, PUT is idempotent.

### Example: POST Request (Not Idempotent)

```http
POST /users
```

Request Body:

```json
{
  "name": "Yash"
}
```

First request:

```text
User 101 created
```

Second identical request:

```text
User 102 created
```

Each request creates a new resource. Since the outcome changes every time, POST is not idempotent.
## Relation to orchestrator/main.py

The REST principles discussed above are directly applicable to files such as `orchestrator/main.py`. Resources are exposed through endpoints, HTTP verbs define the operations performed on those resources, statelessness allows each request to be processed independently, and idempotency ensures that retrying requests does not create inconsistent data or unexpected side effects. These principles help developers build predictable and reliable APIs.

## Conclusion

REST principles provide a structured approach to API design. Resources represent the data being managed, while endpoints provide access to those resources. HTTP verbs define the operations that can be performed, and understanding their safety and idempotency characteristics helps developers build predictable systems. Statelessness ensures that every request is self-contained, making applications more scalable and reliable. Together, these principles form the foundation of modern RESTful API development and are essential knowledge for any developer working with web services.
