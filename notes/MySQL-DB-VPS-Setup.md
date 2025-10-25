# 🍃 Setup MySQL on VPS (Ubuntu / Hostinger)

<p align="center">
  <em>Install, configure, and secure MySQL on your VPS — ready for any application.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white"/>
  <img src="https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white"/>
  <img src="https://img.shields.io/badge/Hostinger-673DE6?style=for-the-badge&logo=hostinger&logoColor=white"/>
  <img src="https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black"/>
  <img src="https://img.shields.io/badge/Terminal-000000?style=for-the-badge&logo=gnometerminal&logoColor=white"/>
</p>

---

## 🧰 Prerequisites

Make sure you have:

* 🖥️ A **VPS** (e.g., Hostinger, AWS, DigitalOcean)  
* 🔑 **SSH access** to your VPS  
* ⚙️ **Ubuntu 22.04 or later**  
* 👨‍💻 **Sudo privileges**

---

## ⚙️ 1️⃣ Install MySQL

### Step 1: Update Package Index

```bash
sudo apt-get update
````

### Step 2: Install MySQL Server

```bash
sudo apt-get install -y mysql-server
```

### Step 3: Verify Installation

```bash
mysql --version
```

✅ You should see the installed MySQL version.

---

## ⚙️ 2️⃣ Start & Enable MySQL Service

```bash
sudo systemctl start mysql
```
```bash
sudo systemctl enable mysql
```

### Verify Status

```bash
sudo systemctl status mysql
```

✅ Look for `active (running)` in green.

---

## 🔒 3️⃣ Secure MySQL Installation

Run the security script:

```bash
sudo mysql_secure_installation
```

Follow prompts to:

* Set a **root password**
* Remove **anonymous users**
* Disallow **remote root login**
* Remove the **test database**
* Reload **privileges**

---

## 👑 4️⃣ Log in as Root User

```bash
sudo mysql -u root -p
```

Enter the root password set during the secure installation.

---

## 🗄️ 5️⃣ Create a Database & User

### Step 1: Create a New Database

```sql
CREATE DATABASE myapp_db;
```

### Step 2: Create a New User

```sql
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypassword';
```

### Step 3: Grant Privileges

```sql
GRANT ALL PRIVILEGES ON myapp_db.* TO 'myuser'@'localhost';
```

### Step 4: Flush Privileges

```sql
FLUSH PRIVILEGES;
```

### Step 5: Exit MySQL

```sql
EXIT;
```

---

## 🔗 6️⃣ Setup MySQL URL for Node.js / Express / Next.js

Create a `.env` file in your project root:

```env
MYSQL_URI=mysql://myuser:mypassword@127.0.0.1:3306/myapp_db
```

### Example Usage with Node.js + Sequelize

```js
require('dotenv').config();
const { Sequelize } = require('sequelize');

const sequelize = new Sequelize(process.env.MYSQL_URI, {
  dialect: 'mysql',
  logging: false,
});

sequelize.authenticate()
  .then(() => console.log('MySQL connected successfully!'))
  .catch(err => console.error('Unable to connect to MySQL:', err));
```

### Example Usage with Node.js + mysql2

```js
require('dotenv').config();
const mysql = require('mysql2');

const connection = mysql.createConnection(process.env.MYSQL_URI);

connection.connect(err => {
  if (err) {
    console.error('Error connecting to MySQL:', err);
    return;
  }
  console.log('Connected to MySQL!');
});
```

> 💡 **Tip:** Keep your `.env` file secret. Never commit it to GitHub.

---

## 🧠 Useful Commands

| Command                        | Description           |
| ------------------------------ | --------------------- |
| `sudo systemctl start mysql`   | Start MySQL           |
| `sudo systemctl stop mysql`    | Stop MySQL            |
| `sudo systemctl restart mysql` | Restart MySQL         |
| `sudo systemctl status mysql`  | Check MySQL status    |
| `mysql -u root -p`             | Log in as root        |
| `sudo ufw allow 3306`          | Allow MySQL port 3306 |

---

## ✅ Summary

🎉 Congratulations! You have successfully installed and secured **MySQL** on your VPS.

You can now:

* Create databases and users
* Connect Node.js / Express / Next.js apps to MySQL
* Manage MySQL securely via the terminal

---

## 👨‍💻 Author

**Muzaffar Khan**  
GitHub: [https://github.com/muzaffar-khan](https://github.com/muzaffar-khan)  

> 💬 “A properly configured database is the backbone of every reliable web app.”
