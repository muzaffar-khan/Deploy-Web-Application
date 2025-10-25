<h1 align="center">🍃 Setup MongoDB on VPS (Ubuntu / Hostinger)</h1>

<p align="center">
  <em>Install, configure, and secure MongoDB on your VPS — ready for MERN or Next.js apps.</em>
</p>

<p align="center">
  <!-- Tech Badges -->
  <img src="https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white"/>
  <img src="https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white"/>
  <img src="https://img.shields.io/badge/Hostinger-673DE6?style=for-the-badge&logo=hostinger&logoColor=white"/>
  <img src="https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black"/>
  <img src="https://img.shields.io/badge/Terminal-000000?style=for-the-badge&logo=gnometerminal&logoColor=white"/>
</p>

## 

## 🧰 Prerequisites

Before starting, make sure you have:
- 🖥️ A **VPS** (e.g., Hostinger, AWS or DigitalOcean)
- 🔑 **SSH access** to your VPS
- ⚙️ **Ubuntu 22.04 or later**
- 👨‍💻 **Sudo privileges**

## 
## ⚙️ 1️⃣ Install MongoDB

### Step 1: Install Dependencies
```bash
sudo apt-get install gnupg curl
````

### Step 2: Import MongoDB GPG Key

```bash
curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | \
  sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg --dearmor
```

### Step 3: Create MongoDB List File

```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/8.0 multiverse" | \
  sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list
```

### Step 4: Update & Install MongoDB

```bash
sudo apt-get update
```
```bash
sudo apt-get install -y mongodb-org
```

---

## ⚙️ 2️⃣ Start & Enable MongoDB Service

Reload and enable MongoDB to start automatically on boot:

```bash
sudo systemctl daemon-reload
```
```bash
sudo systemctl start mongod
```
```bash
sudo systemctl enable mongod
```

### Verify Status

```bash
sudo systemctl status mongod
```

✅ You should see `active (running)` in green.

---

## 🧩 3️⃣ MongoDB Connection URI

Default local connection string:

```bash
mongodb://127.0.0.1:27017
```

Use this URI inside your `.env` file for local connections:

```env
MONGO_URI=mongodb://127.0.0.1:27017
```

---

## 🔒 4️⃣ Configure Firewall (Optional)

Enable firewall:

```bash
sudo ufw enable
```

Check status:

```bash
sudo ufw status
```

If MongoDB port `27017` isn’t allowed:

```bash
sudo ufw allow 27017
```
```bash
sudo ufw allow 'OpenSSH'
```

Restart MongoDB:

```bash
sudo service mongod restart
```

---

## 👑 5️⃣ Secure MongoDB with Authentication

### Step 1: Access Mongo Shell

```bash
mongosh
```

### Step 2: Switch to Admin Database

```bash
use admin
```

### Step 3: Create a Superuser

```bash
db.createUser({
  user: "username-here",
  pwd: passwordPrompt(),
  roles: ["root"]
})
```

### Step 4: Exit Mongo Shell

```bash
.exit
```

---

## 🔐 6️⃣ Enable Authorization

Open MongoDB configuration file:

```bash
sudo nano /etc/mongod.conf
```

Find and modify:

```yaml
security:
  authorization: enabled
```

Save & exit (`Ctrl + X`, then `Y`, and `Enter`).

Restart MongoDB:

```bash
sudo service mongod restart
```

---

## 👤 7️⃣ Connect as Superuser

```bash
mongosh --port 27017 --authenticationDatabase "admin" -u "username-here" -p "password-here"
```

---

## 🗄️ 8️⃣ Create Database & Project User

Switch to your app’s database:

```bash
use database_name
```

Create a project user with read/write access:

```bash
db.createUser({
  user: "username_here",
  pwd: passwordPrompt(),
  roles: [{ role: "readWrite", db: "database_name" }]
})
```

Exit:

```bash
.exit
```

---

## 🔗 9️⃣ MongoDB URI for Projects

Use this URI in your `.env`:

```bash
mongodb://username-here:password-here@127.0.0.1:27017/database_name
```

Example `.env`:

```env
MONGO_URI=mongodb://myuser:mypassword@127.0.0.1:27017/myapp
```

---

## 🧠 Useful Commands

| Command                         | Description          |
| ------------------------------- | -------------------- |
| `sudo systemctl start mongod`   | Start MongoDB        |
| `sudo systemctl stop mongod`    | Stop MongoDB         |
| `sudo systemctl restart mongod` | Restart MongoDB      |
| `sudo systemctl status mongod`  | Check MongoDB status |
| `mongosh`                       | Open Mongo shell     |
| `sudo ufw status`               | Check firewall rules |

---

## ✅ Summary

🎉 Congratulations! You’ve successfully installed and secured **MongoDB** on your VPS.

Now you can:

* Connect your **Node.js / Express / Next.js** apps
* Use **PM2 + Nginx** for production hosting
* Store your data safely on a **secured local database**

---

## 👨‍💻 Author

**Author:** Muzaffar Khan  
**GitHub:** [https://github.com/muzaffar-khan](https://github.com/muzaffar-khan)  
> 💬 “A properly configured database is the backbone of every reliable web app.”

---

<p align="center">
  ⭐ <b>Star this repo</b> if this helped you — it motivates me to create more guides!  
</p>

<p align="center">
  <img src="https://img.shields.io/github/stars/muzaffar-khan/Deploying-Web-Application-on-VPS?style=social" alt="GitHub stars">
</p>
