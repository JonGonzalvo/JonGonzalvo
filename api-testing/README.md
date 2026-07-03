# API Testing

This folder contains Postman collections demonstrating REST API testing skills.

## Collections

| File | API | # of Requests |
|---|---|---|
| `postman-collections/reqres-api-tests.json` | ReqRes.in (public demo API) | 12 |

## How to Import

1. Open Postman
2. Click **Import** (top left)
3. Drag the `.json` file into the import dialog
4. Click **Import**
5. The collection appears in your left sidebar — click Run to execute all requests

## What's Tested

Using [ReqRes.in](https://reqres.in) — a publicly available REST API for testing:

- **GET /users** — List users, verify 200 response and correct data structure
- **GET /users/:id** — Single user lookup (valid and invalid IDs)
- **POST /users** — Create a user, verify 201 and response body
- **PUT /users/:id** — Full update, verify fields in response
- **PATCH /users/:id** — Partial update
- **DELETE /users/:id** — Verify 204 No Content
- **POST /login** — Successful auth, verify token returned
- **POST /login** — Failed auth (missing password), verify 400 + error message
- **POST /register** — Successful registration
- **POST /register** — Failed registration (missing fields)

## Testing Approach

Each request includes:
- **Pre-request scripts** — Set up test data or tokens
- **Tests (Assertions)** — Status code, response body, data type checks
- **Environment variables** — Base URL and auth tokens stored as variables, not hardcoded

## Example Test Script (Postman)

```javascript
// Verify status code
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

// Verify response structure
pm.test("Response has user data", function () {
    const json = pm.response.json();
    pm.expect(json.data).to.have.property("id");
    pm.expect(json.data).to.have.property("email");
    pm.expect(json.data.email).to.be.a("string");
});

// Verify response time
pm.test("Response time under 1000ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(1000);
});
```
