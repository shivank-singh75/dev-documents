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
