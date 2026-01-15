Basic CRUD API with Node.js and Express

---

Repository structure

`
basic-crud-api/
├─ index.js
├─ package.json
├─ README.md
├─ .gitignore
├─ data.json
└─ postman_collection.json
`

---

Files and code

index.js

`js
const express = require('express');
const app = express();
const PORT = 3000;

// In-memory data store (can be replaced with file/db later)
let users = [
  { id: 1, name: 'Annu', age: 22 },
  { id: 2, name: 'Ravi', age: 25 }
];

app.use(express.json());

// Health check
app.get('/', (req, res) => {
  res.json({ message: 'Basic CRUD API is running', version: '1.0.0' });
});

// Create: Add new user
app.post('/users', (req, res) => {
  const { name, age } = req.body;

  if (!name || typeof age !== 'number') {
    return res.status(400).json({ error: 'Invalid payload. Provide name (string) and age (number).' });
  }

  const id = users.length ? Math.max(...users.map(u => u.id)) + 1 : 1;
  const newUser = { id, name, age };
  users.push(newUser);

  res.status(201).json({ message: 'User created', data: newUser });
});

// Read: Get all users
app.get('/users', (req, res) => {
  res.json({ count: users.length, data: users });
});

// Read: Get user by ID
app.get('/users/:id', (req, res) => {
  const id = Number(req.params.id);
  const user = users.find(u => u.id === id);

  if (!user) return res.status(404).json({ error: 'User not found' });

  res.json({ data: user });
});

// Update: Update user by ID
app.put('/users/:id', (req, res) => {
  const id = Number(req.params.id);
  const { name, age } = req.body;

  const index = users.findIndex(u => u.id === id);
  if (index === -1) return res.status(404).json({ error: 'User not found' });

  if (name !== undefined) users[index].name = name;
  if (age !== undefined) {
    if (typeof age !== 'number') return res.status(400).json({ error: 'Age must be a number' });
    users[index].age = age;
  }

  res.json({ message: 'User updated', data: users[index] });
});

// Delete: Remove user by ID
app.delete('/users/:id', (req, res) => {
  const id = Number(req.params.id);
  const exists = users.some(u => u.id === id);

  if (!exists) return res.status(404).json({ error: 'User not found' });

  users = users.filter(u => u.id !== id);
  res.json({ message: User with id ${id} deleted });
});

// Start server
app.listen(PORT, () => {
  console.log(Server running on http://localhost:${PORT});
});
`

---

package.json

`json
{
  "name": "basic-crud-api",
  "version": "1.0.0",
  "description": "Beginner-friendly CRUD API using Node.js and Express",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  },
  "keywords": ["node", "express", "crud", "api", "beginner"],
  "author": "Annu",
  "license": "MIT",
  "dependencies": {
    "express": "^4.19.2"
  },
  "devDependencies": {
    "nodemon": "^3.0.2"
  }
}
`

---

.gitignore

`
node_modules
.env
.DS_Store
`

---

data.json

`json
[
  { "id": 1, "name": "Annu", "age": 22 },
  { "id": 2, "name": "Ravi", "age": 25 }
]
`

(Note: The app uses in-memory data for simplicity. This file is just for reference or future extension.)

---

postman_collection.json

`json
{
  "info": {
    "name": "Basic CRUD API Collection",
    "postmanid": "b1f0f0a1-1234-5678-9abc-def012345678",
    "description": "Postman collection to test CRUD endpoints",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
  },
  "item": [
    {
      "name": "Health Check",
      "request": { "method": "GET", "url": "http://localhost:3000/" }
    },
    {
      "name": "Get All Users",
      "request": { "method": "GET", "url": "http://localhost:3000/users" }
    },
    {
      "name": "Get User by ID",
      "request": { "method": "GET", "url": "http://localhost:3000/users/1" }
    },
    {
      "name": "Create User",
      "request": {
        "method": "POST",
        "header": [{ "key": "Content-Type", "value": "application/json" }],
        "body": { "mode": "raw", "raw": "{\n  \"name\": \"Neha\",\n  \"age\": 24\n}" },
        "url": "http://localhost:3000/users"
      }
    },
    {
      "name": "Update User",
      "request": {
        "method": "PUT",
        "header": [{ "key": "Content-Type", "value": "application/json" }],
        "body": { "mode": "raw", "raw": "{\n  \"name\": \"Annu Updated\",\n  \"age\": 23\n}" },
        "url": "http://localhost:3000/users/1"
      }
    },
    {
      "name": "Delete User",
      "request": { "method": "DELETE", "url": "http://localhost:3000/users/2" }
    }
  ]
}
`

---

README.md

`md

Basic CRUD API (Beginner Level)

A simple Node.js + Express API demonstrating CRUD operations with an in-memory data store. Perfect for learning the request–response cycle, testing with Postman, and documenting your work.

Tech stack
- Node.js
- Express.js
- JavaScript
- Postman (for testing)

Project setup
1. Clone or create folder
2. Initialize project
   `bash
   npm init -y
   `
3. Install dependencies
   `bash
   npm install express
   npm install --save-dev nodemon
   `
4. Run server
   `bash
   npm run start

or for auto-reload
   npm run dev
   `
   Server runs on http://localhost:3000.

API endpoints

- Health: GET /  
  Returns API status.

- Create: POST /users  
  Body: { "name": "string", "age": number }

- Read all: GET /users

- Read by ID: GET /users/:id

- Update by ID: PUT /users/:id  
  Body (optional fields): { "name": "string", "age": number }

- Delete by ID: DELETE /users/:id

Example responses
- Create (201):
  `json
  { "message": "User created", "data": { "id": 3, "name": "Neha", "age": 24 } }
  `
- Not found (404):
  `json
  { "error": "User not found" }
  `
- Bad request (400):
  `json
  { "error": "Invalid payload. Provide name (string) and age (number)." }
  `

Testing with Postman
- Import postman_collection.json into Postman.
- Run each request in order: Health → Get All → Create → Get by ID → Update → Delete.

Report (Assignment guidelines)

Setup steps
- Initialized Node.js with npm init -y
- Installed Express and Nodemon
- Created index.js, package.json, .gitignore, README.md
- Verified server on port 3000

API explanation
- POST /users: Adds a new user with auto-incremented id
- GET /users: Returns all users with count
- GET /users/:id: Returns a single user by id
- PUT /users/:id: Updates name and/or age for a user
- DELETE /users/:id: Removes user by id

Problems faced and solutions
- Validation: Ensured age is a number and name is present → returned 400 on invalid payload
- ID management: Used Math.max(...ids) + 1 to generate next id
- State management: Kept in-memory array for simplicity; noted future move to DB/file

Learnings
- Express routing and middleware (express.json())
- RESTful design and status codes (200/201/400/404)
- Postman testing workflow and request bodies
- Importance of clear, beginner-friendly documentation

Notes
This is intentionally simple for learning. You can extend it with:
- File-based persistence (read/write data.json)
- Database (MongoDB/PostgreSQL)
- Validation libraries (Joi/Zod)
- Router modules and controllers
