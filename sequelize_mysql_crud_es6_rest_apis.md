# Sequelize + MySQL CRUD Guide (Using ES6 and Raw Queries)

This step-by-step guide walks you through setting up Sequelize with MySQL and performing CRUD operations using raw SQL queries in ES6 syntax.

---

## 📦 1. Prerequisites

Make sure you have installed:

- Node.js
- MySQL Server
- Code editor (e.g., VS Code)

---

## 🏗️ 2. Initialize Node.js Project

```bash
mkdir sequelize-mysql-crud
cd sequelize-mysql-crud
npm init -y
```

---

## 📥 3. Install Dependencies

```bash
npm install sequelize mysql2
```

---

## 🧩 4. Configure Database Connection (`db.js`)

```js
import { Sequelize } from 'sequelize';

const sequelize = new Sequelize('your_db_name', 'your_db_user', 'your_db_password', {
  host: 'localhost',
  dialect: 'mysql',
});

export default sequelize;
```

---

## ✅ 5. Test Database Connection (`index.js`)

```js
import sequelize from './db.js';

const testConnection = async () => {
  try {
    await sequelize.authenticate();
    console.log('✅ Connected to MySQL database!');
  } catch (error) {
    console.error('❌ Connection error:', error);
  }
};

testConnection();
```

---

## 🧱 6. Create a Table

```js
await sequelize.query(`
  CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE
  );
`);
```

---

## ✍️ 7. Insert a User

```js
await sequelize.query(
  'INSERT INTO users (name, email) VALUES (:name, :email)',
  {
    replacements: { name: 'John Doe', email: 'john@example.com' }
  }
);
```

---

## 📄 8. Read Users

```js
const [users] = await sequelize.query('SELECT * FROM users');
console.log(users);
```

---

## 🔁 9. Update a User

```js
await sequelize.query(
  'UPDATE users SET name = :newName WHERE email = :email',
  {
    replacements: { newName: 'Jane Doe', email: 'john@example.com' }
  }
);
```

---

## ❌ 10. Delete a User

```js
await sequelize.query(
  'DELETE FROM users WHERE email = :email',
  {
    replacements: { email: 'john@example.com' }
  }
);
```

---

## 🧪 11. Full Example (`index.js`)

```js
import sequelize from './db.js';

const main = async () => {
  try {
    await sequelize.authenticate();
    console.log('✅ Connected to DB');

    await sequelize.query(\`
      CREATE TABLE IF NOT EXISTS users (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(100),
        email VARCHAR(100) UNIQUE
      );
    \`);

    await sequelize.query(
      'INSERT INTO users (name, email) VALUES (:name, :email)',
      { replacements: { name: 'John Doe', email: 'john@example.com' } }
    );

    const [users] = await sequelize.query('SELECT * FROM users');
    console.log(users);

    await sequelize.query(
      'UPDATE users SET name = :newName WHERE email = :email',
      { replacements: { newName: 'Jane Doe', email: 'john@example.com' } }
    );

    await sequelize.query(
      'DELETE FROM users WHERE email = :email',
      { replacements: { email: 'john@example.com' } }
    );

    console.log('✅ CRUD operations completed!');
  } catch (err) {
    console.error('❌ Error:', err);
  }
};

main();
```

---

## 📚 Done!

You now have a working Sequelize + MySQL CRUD setup using ES6 and raw SQL queries.


---

## 🚀 12. Create REST APIs with Express

### 📦 Install Express

```bash
npm install express
```

---

### 🔌 API Setup (`server.js`)

```js
import express from 'express';
import sequelize from './db.js';

const app = express();
app.use(express.json());

// Create User
app.post('/users', async (req, res) => {
  const { name, email } = req.body;
  try {
    await sequelize.query(
      'INSERT INTO users (name, email) VALUES (:name, :email)',
      { replacements: { name, email } }
    );
    res.status(201).send({ message: 'User created' });
  } catch (err) {
    res.status(500).send({ error: err.message });
  }
});

// List All Users
app.get('/users', async (req, res) => {
  try {
    const [users] = await sequelize.query('SELECT * FROM users');
    res.send(users);
  } catch (err) {
    res.status(500).send({ error: err.message });
  }
});

// Get User by ID
app.get('/users/:id', async (req, res) => {
  try {
    const [user] = await sequelize.query(
      'SELECT * FROM users WHERE id = :id',
      { replacements: { id: req.params.id } }
    );
    if (user.length === 0) return res.status(404).send({ message: 'User not found' });
    res.send(user[0]);
  } catch (err) {
    res.status(500).send({ error: err.message });
  }
});

// Update User
app.put('/users/:id', async (req, res) => {
  const { name, email } = req.body;
  try {
    await sequelize.query(
      'UPDATE users SET name = :name, email = :email WHERE id = :id',
      { replacements: { name, email, id: req.params.id } }
    );
    res.send({ message: 'User updated' });
  } catch (err) {
    res.status(500).send({ error: err.message });
  }
});

// Delete User
app.delete('/users/:id', async (req, res) => {
  try {
    await sequelize.query(
      'DELETE FROM users WHERE id = :id',
      { replacements: { id: req.params.id } }
    );
    res.send({ message: 'User deleted' });
  } catch (err) {
    res.status(500).send({ error: err.message });
  }
});

// Start server
app.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```

---

### ✅ You now have full REST API support for CRUD operations with Sequelize (using raw queries and ES6)!
