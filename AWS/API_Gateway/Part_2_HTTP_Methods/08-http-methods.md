# Part 2: HTTP Methods

HTTP methods describe the **action the client wants to perform on a resource**.

---

## 1. GET — Read

Used to **retrieve data**.

```http
GET /users
```

Get a specific user:

```http
GET /users/101
```

**GET → "Give me this data"**

---

## 2. POST — Create

Used to **create a new resource**.

```http
POST /users
Content-Type: application/json

{
  "name": "John"
}
```

**POST → "Create something new"**

---

## 3. PUT — Replace

Used to **replace/update an entire resource**.

```http
PUT /users/101

{
  "name": "John",
  "age": 30,
  "city": "Mumbai"
}
```

**PUT → "Replace the resource with this"**

---

## 4. PATCH — Partial Update

Used to **update only specific fields**.

```http
PATCH /users/101

{
  "city": "Pune"
}
```

Only the `city` needs to change.

**PATCH → "Change only this part"**

---

## 5. DELETE — Delete

Used to **remove a resource**.

```http
DELETE /users/101
```

**DELETE → "Remove this resource"**

---

## 6. OPTIONS — What is allowed?

Used to determine what methods/options are supported.

It is especially important for **CORS preflight requests** made by browsers.

Example:

```http
OPTIONS /users
```

The server may respond with information such as:

```text
Allowed Methods:
GET, POST, PUT, DELETE
```

**OPTIONS → "Ask what's allowed"**

---

## Quick Comparison

| Method | Action | Example |
|---|---|---|
| GET | Read | `GET /users/101` |
| POST | Create | `POST /users` |
| PUT | Replace | `PUT /users/101` |
| PATCH | Partial update | `PATCH /users/101` |
| DELETE | Delete | `DELETE /users/101` |
| OPTIONS | Check allowed options | `OPTIONS /users` |

---

## Easy Memory Trick

```text
GET     → Read
POST    → Create
PUT     → Replace
PATCH   → Modify
DELETE  → Remove
OPTIONS → Ask what's allowed
```

---

## Key Takeaways

- HTTP methods define what action the client wants to perform.
- GET retrieves data.
- POST creates a resource.
- PUT replaces a resource.
- PATCH partially updates a resource.
- DELETE removes a resource.
- OPTIONS is commonly involved in CORS preflight requests.
