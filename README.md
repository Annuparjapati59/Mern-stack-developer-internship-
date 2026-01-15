 CRUD API

1. Project Setup
- Create a folder: basic-crud-api
- Initialize Node.js:  
  `bash
  npm init -y
  `
- Install Express:  
  `bash
  npm install express
  `

---

2. Project Structure
`
basic-crud-api/
 ├── index.js        # Main server file
 ├── package.json    # Dependencies & scripts
 └── README.md       # Documentation
`

---

3. Code (index.js)
Here’s a clean starter code for your CRUD API:

`javascript
const express = require("express");
const app = express();
const PORT = 3000;

app.use(express.json());

// Temporary in-memory data
let users = [
  { id: 1, name: "Annu", age: 22 },
  { id: 2, name: "Rahul", age: 25 }
];

// CREATE
app.post("/users", (req, res) => {
  const newUser = req.body;
  users.push(newUser);
  res.status(201).json(newUser);
});

// READ (all)
app.get("/users", (req, res) => {
  res.json(users);
});

// READ (by ID)
app.get("/users/:id", (req, res) => {
  const user = users.find(u => u.id == req.params.id);
  user ? res.json(user) : res.status(404).send("User not found");
});

// UPDATE
app.put("/users/:id", (req, res) => {
  const index = users.findIndex(u => u.id == req.params.id);
  if (index !== -1) {
    users[index] = { ...users[index], ...req.body };
    res.json(users[index]);
  } else {
    res.status(404).send("User not found");
  }
});

// DELETE
app.delete("/users/:id", (req, res) => {
  users = users.filter(u => u.id != req.params.id);
  res.send("User deleted");
});

app.listen(PORT, () => {
  console.log(Server running on http://localhost:${PORT});
});
`

---

4. Run the Server
`bash
node index.js
`
Server will run at: http://localhost:3000

---

5. Test with Postman
- POST → http://localhost:3000/users  
- GET → http://localhost:3000/users  
- GET by ID → http://localhost:3000/users/1  
- PUT → http://localhost:3000/users/1  
- DELETE → http://localhost:3000/users/1

---

 README 

`markdown

Basic CRUD API 

Objective
A simple CRUD (Create, Read, Update, Delete) API built with Node.js and Express.  

---

Tech Stack
- Node.js
- Express.js
- JavaScript
- Postman (for testing)

---

Setup Instructions
1. Clone the repository:
   `bash
   git clone https://github.com/your-username/basic-crud-api.git
   `
2. Navigate into the folder:
   `bash
   cd basic-crud-api
   `
3. Install dependencies:
   `bash
   npm install
   `
4. Start the server:
   `bash
   node index.js
   `

---

 Project Structure
`
basic-crud-api/
 ├── index.js        # Main server file
 ├── package.json    # Dependencies & scripts
 └── README.md       # Documentation
`

---

API Endpoints
| Method | Endpoint         | Description          |
|--------|------------------|----------------------|
| POST   | /users           | Create new user      |
| GET    | /users           | Get all users        |
| GET    | /users/:id       | Get user by ID       |
| PUT    | /users/:id       | Update user by ID    |
| DELETE | /users/:id       | Delete user by ID    |

---

 Testing
Use Postman to test all endpoints:
- Create → POST /users
- Read → GET /users and GET /users/:id
- Update → PUT /users/:id
- Delete → DELETE /users/:id

---



 Add Postman Screenshots in README

1. Test each API endpoint in Postman  
   - Run your server (node index.js)  
   - Open Postman and send requests for:
     - POST /users → Create user  
     - GET /users → Get all users  
     - GET /users/:id → Get user by ID  
     - PUT /users/:id → Update user  
     - DELETE /users/:id → Delete user  

2. Take screenshots  
   - Capture the request + response window in Postman.  
   - Save them as PNG/JPG files (e.g., postman-create.png, postman-get.png).  

3. Add screenshots to your GitHub repo  
   - Place them in a folder called assets/ inside your project.  
   - Example structure:
     `
     basic-crud-api/
      ├── index.js
      ├── package.json
      ├── README.md
      └── assets/
          ├── postman-create.png
          ├── postman-get.png
          ├── postman-update.png
          └── postman-delete.png
     `

4. Reference screenshots in README  
   Use Markdown image syntax:
   `markdown

 API Testing Screenshots

Create User (POST /users)
   !Create User

Get All Users (GET /users)
   !Get Users

Update User (PUT /users/:id)
   !Update User

Delete User (DELETE /users/:id)
   !Delete User
   `

---

Example README Section with Screenshots

`markdown

 API Testing Screenshots

Create User (POST /users)
!Create User

Get All Users (GET /users)
!Get Users

Update User (PUT /users/:id)
!Update User

Delete User (DELETE /users/:id)
!Delete User

