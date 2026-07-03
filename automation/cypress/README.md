# Cypress Automation

Basic smoke test suite written in Cypress for the demo e-commerce application.

## Setup

```bash
# Install dependencies
npm install

# Open Cypress UI
npx cypress open

# Run headless
npx cypress run
```

## What's Covered

| Test File | Module | Tests |
|---|---|---|
| `login.cy.js` | Authentication | Valid login, invalid login, empty fields |
| `search.cy.js` | Product Search | Keyword search, no results, case insensitivity |
| `cart.cy.js` | Cart | Add to cart, update quantity, remove item |

## Project Structure

```
cypress/
├── e2e/
│   ├── login.cy.js
│   ├── search.cy.js
│   └── cart.cy.js
├── fixtures/
│   └── users.json          ← test credentials stored here
├── support/
│   ├── commands.js         ← custom commands (e.g., cy.login())
│   └── e2e.js
└── cypress.config.js
```

## Sample Test

```javascript
// login.cy.js
describe('User Authentication', () => {
  beforeEach(() => {
    cy.visit('/login')
  })

  it('logs in with valid credentials', () => {
    cy.fixture('users').then((users) => {
      cy.get('#email').type(users.valid.email)
      cy.get('#passwd').type(users.valid.password)
      cy.get('#SubmitLogin').click()
      cy.url().should('include', '/my-account')
      cy.get('.account').should('be.visible')
    })
  })

  it('shows error with wrong password', () => {
    cy.get('#email').type('test@example.com')
    cy.get('#passwd').type('wrongpassword')
    cy.get('#SubmitLogin').click()
    cy.get('.alert').should('contain', 'Authentication failed')
  })

  it('shows validation error when fields are empty', () => {
    cy.get('#SubmitLogin').click()
    cy.get('.alert').should('be.visible')
  })
})
```

## Custom Command Example

```javascript
// support/commands.js
Cypress.Commands.add('login', (email, password) => {
  cy.visit('/login')
  cy.get('#email').type(email)
  cy.get('#passwd').type(password)
  cy.get('#SubmitLogin').click()
  cy.url().should('include', '/my-account')
})
```

Then use it in any test:
```javascript
cy.login('user@example.com', 'password123')
```
