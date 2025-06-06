# store-and-retrieve-data-in-postgre-sql
Express.js and PostgreSQL CRUD API
This project provides a simple RESTful API built with Node.js and Express.js that performs CRUD (Create, Read, Update, Delete) operations on a PostgreSQL database.

Prerequisites
Node.js (which includes npm)

PostgreSQL

Project Setup Instructions
1. Install Dependencies
First, save the application code as index.js in a new directory. Then, navigate to your project directory in the terminal and run:

# Initialize a new Node.js project
npm init -y

# Install required packages
npm install express pg

2. Set Up PostgreSQL Database
You need to create a database and a users table for the application to use.

a. Log in to PostgreSQL:
Open your terminal and log in to the PostgreSQL interactive terminal (psql).

b. Create a Database:
Create a new database. We'll call it api_db for this example.

CREATE DATABASE api_db;

c. Connect to the New Database:
Quit psql (\q) and reconnect to your newly created database.

psql -d api_db

d. Create the users Table:
Run the following SQL command to create the necessary table.

CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL,
  age INTEGER
);

3. Configure Database Connection
Open the index.js file and find the Pool configuration section. Update the credentials to match your local PostgreSQL setup.

// From index.js
const pool = new Pool({
    user: 'postgres',          // Your PostgreSQL username
    host: 'localhost',
    database: 'api_db',        // The database you just created
    password: 'your_password', // <-- IMPORTANT: Change this!
    port: 5432,
});

4. Run the Application
Once the database is set up and the connection is configured, you can start the server.

node index.js

The server will start on http://localhost:3000.

API Documentation
Test the API using a tool like Postman or cURL.

GET /users
Description: Get all users.

cURL: curl http://localhost:3000/users

Success Response (200 OK):

[
  {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com",
    "age": 30
  }
]

GET /users/:id
Description: Get a single user by their ID.

cURL: curl http://localhost:3000/users/1

Success Response (200 OK):

{
  "id": 1,
  "name": "John Doe",
  "email": "john.doe@example.com",
  "age": 30
}

Error Response (404 Not Found): {"message": "User not found."}

POST /users
Description: Create a new user.

cURL:

curl -X POST -H "Content-Type: application/json" -d '{"name":"Jane Smith", "email":"jane.smith@example.com", "age":28}' http://localhost:3000/users

Success Response (201 Created):

{
  "id": 2,
  "name": "Jane Smith",
  "email": "jane.smith@example.com",
  "age": 28
}

Error Response (400 Bad Request): {"message": "Bad Request. Name, email, and age are required."}

PUT /users/:id
Description: Update an existing user.

cURL:

curl -X PUT -H "Content-Type: application/json" -d '{"name":"Jane Smith", "email":"jane.s@example.com", "age":29}' http://localhost:3000/users/2

Success Response (200 OK):

{
  "id": 2,
  "name": "Jane Smith",
  "email": "jane.s@example.com",
  "age": 29
}

Error Response (404 Not Found): {"message": "User not found."}

DELETE /users/:id
Description: Delete a user.

cURL: curl -X DELETE http://localhost:3000/users/1

Success Response (200 OK): {"message": "User deleted successfully."}

Error Response (404 Not Found): {"message": "User not found."}
