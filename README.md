Basic CRUD API with Node.js + Express 

Project structure

`
basic-crud-api/
├─ index.js
├─ package.json
└─ README.md
`


index.js 

`js
const express = require('express');
const app = express();
const PORT = 3000;

// Middleware to parse JSON bodies
app.use(express.json());

// In-memory data store
let users = [
  { id: 1, name: 'Annu', age: 22 },
  { id: 2, name: 'Ravi', age: 24 }
];

// Health check
app.get('/', (req, res) => {
  res.json({ message: 'Basic CRUD API is running', version: '1.0.0' });
});

// CREATE: Add new user
app.post('/users', (req, res) => {
  const { name, age } = req.body;

  // Basic validation
  if (!name || typeof age !== 'number') {
    return res.status(400).json({ error: 'Invalid payload: name (string) and age (number) are required' });
  }

  const newUser = {
    id: users.length ? users[users.length - 1].id + 1 : 1,
    name,
    age
  };

  users.push(newUser);
  res.status(201).json({ message: 'User created', data: newUser });
});

// READ: Get all users
app.get('/users', (req, res) => {
  res.json({ count: users.length, data: users });
});

// READ: Get user by ID
app.get('/users/:id', (req, res) => {
  const id = Number(req.params.id);
  const user = users.find(u => u.id === id);

  if (!user) return res.status(404).json({ error: 'User not found' });
  res.json({ data: user });
});

// UPDATE: Update user by ID
app.put('/users/:id', (req, res) => {
  const id = Number(req.params.id);
  const { name, age } = req.body;

  const index = users.findIndex(u => u.id === id);
  if (index === -1) return res.status(404).json({ error: 'User not found' });

  // Allow partial updates but keep validation simple
  if (name !== undefined && typeof name !== 'string') {
    return res.status(400).json({ error: 'Invalid name: must be a string' });
  }
  if (age !== undefined && typeof age !== 'number') {
    return res.status(400).json({ error: 'Invalid age: must be a number' });
  }

  users[index] = {
    ...users[index],
    ...(name !== undefined ? { name } : {}),
    ...(age !== undefined ? { age } : {})
  };

  res.json({ message: 'User updated', data: users[index] });
});

// DELETE: Remove user by ID
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

Postman testing 

- GET all users
  - Method: GET
  - URL: http://localhost:3000/users

- GET user by ID
  - Method: GET
  - URL: http://localhost:3000/users/1

- CREATE user
  - Method: POST
  - URL: http://localhost:3000/users
  - Body (JSON):
    `json
    { "name": "Priya", "age": 21 }
    `

- UPDATE user
  - Method: PUT
  - URL: http://localhost:3000/users/2
  - Body (JSON):
    `json
    { "name": "Ravi Kumar", "age": 25 }
    `

- DELETE user
  - Method: DELETE
  - URL: http://localhost:3000/users/2

---

README.md 

`md

Basic CRUD API (Node.js + Express)

Objective
A CRUD API demonstrating Create, Read, Update, Delete operations using an in-memory data store.

Tech Stack
- Node.js
- Express.js
- JavaScript
- Postman 

Setup
`bash
npm install
node index.js
`
Server: http://localhost:3000

Endpoints
- GET / — Health check
- POST /users — Create user
- GET /users — Read all users
- GET /users/:id — Read user by ID
- PUT /users/:id — Update user by ID
- DELETE /users/:id — Delete user by ID

Data Model
`json
{ "id": number, "name": string, "age": number }
`

Testing (Postman)
- Create:
`json
POST /users
{ "name": "Priya", "age": 21 }
`
- Update:
`json
PUT /users/1
{ "name": "Annu P", "age": 23 }
`

Notes
- In-memory storage only.
- Focus on clarity, validation, and clean responses.
`

---

Report template 

- Setup steps:  
  - Created project, initialized npm, installed Express, added index.js, started server on port 3000.

- API explanation:  
  - Endpoints: /users for CRUD, in-memory array for data, JSON request/response, basic validation and error handling.

- Problems faced and solutions:  
  - Example: JSON parsing error → added express.json() middleware.  
  - ID not found → returned 404 with clear message.  
  - Partial updates → used object spread to merge fields safely.

- Learnings:  
  - Request/response cycle, REST patterns, status codes (201, 400, 404)
