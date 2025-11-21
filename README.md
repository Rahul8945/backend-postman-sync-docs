# Backend Postman Sync 🚀

Automatically sync your backend API routes from Express, NestJS, Fastify and more to Postman with simple code annotations. Never manually update Postman collections again!

> **Note:** This is the documentation repository. The source code is maintained privately.

## 📦 Installation

### Global Installation (Recommended)
```bash
npm install -g backend-postman-sync
```
### Local Installation (as dev dependency)
```bash
npm install --save-dev backend-postman-sync
```
## 🚀 Quick Start

### 1. Get Your Postman API Key

1. Go to **Postman API Keys**
2. Sign in to your Postman account
3. Click icon or manage account
4. Click setting inside manage account
5. Click **API Key"** on right side
6. Click **"Generate API Key"**
7. Copy your `PMAK-...` key

### 2. Set Environment Variable

#### Windows PowerShell
```powershell
$env:POSTMAN_API_KEY="your_pmac_key_here"
```

#### Windows Command Prompt
```cmd
set POSTMAN_API_KEY=your_pmac_key_here
```

#### Linux / Mac
```bash
export POSTMAN_API_KEY=your_pmac_key_here
```
### 3. Initialize Configuration
```bash
backend-postman-sync init
```

#### You'll be asked for:

1. Collection Name: Name for your Postman collection
2. Base URL: Your API base URL (use {{baseUrl}} for Postman variable)
3. Routes Directory: Where your route files are located
4. Folder Names: Names for each route file in Postman

**If not asked anything, keep the below config file manually named `postman-sync.config.js`.**

### 📁 Example `postman-sync.config.js`

```js
module.exports = {
  "collectionName": "My API Collection",
  "baseUrl": "{{baseUrl}}/api",
  "routesDir": "./src/routes",
  "folders": {
    "user.js": "users",    
    "management.js": "User Management", 
    "*": "Other Endpoints"
  }
};
```
#### example

```text
📁 My API Collection
   ├── 📂 Authentication     (from auth.js)
   └── 📂 User Management    (from users.js)
```

### 4. Add Annotations to Your Routes
```text
// routes/auth.js

// @api Register User | POST /auth/register {name, email, password, phone}
router.post('/register', (req, res) => {
  res.json({ message: 'User registered successfully' });
});

// @api Login User | POST /auth/login {email, password}
router.post('/login', (req, res) => {
  res.json({ message: 'Login successful' });
});
```

### 5. Sync to Postman
```bash
backend-postman-sync sync
```

#### After this command, your Postman will look like the folder structure below, generated from your custom file names, paths, and request data (body, params, path).

```text
📁 My API Collection
   ├── 📂 Authentication
   │      ├── Register User      (POST /auth/register)
   │      └── Login User         (POST /auth/login)
```

## 📋 Complete Commands Reference

```bash
backend-postman-sync init
```
#### Initialize configuration for your project.

1. Creates postman-sync.config.js file
2. Asks for collection name, base URL, routes directory
3. Maps route files to Postman folder names
4. Usage: **backend-postman-sync init**

```bash
backend-postman-sync sync
```
#### Scan route files and sync to Postman.

1. Scans all route files for @api annotations
2. Builds Postman collection structure
3. Creates or updates collection in your Postman workspace
4. Shows summary of endpoints synced
5. Usage: **backend-postman-sync sync**

```bash
backend-postman-sync dry-run
```
#### Preview what will be synced without making changes.

1. Shows all endpoints that would be synced
2. Displays request bodies and query parameters
3. No changes made to Postman
4. Perfect for testing annotations
5. Usage: **backend-postman-sync dry-run**

```bash
backend-postman-sync dry-run
```
#### Preview what will be synced without making changes.

1. Shows all endpoints that would be synced
2. Displays request bodies and query parameters
3. No changes made to Postman
4. Perfect for testing annotations
5. Usage: **backend-postman-sync dry-run**

## 🏷️ Complete Annotation Guide

#### Format 1: Simple Syntax

```text
// @api METHOD /path {body_fields} ?query_params
// @api POST /users {name, email, password}
// @api GET /users ?page,limit,search
// @api PUT /users/{id} {name, email}
// @api DELETE /users/{id}
// @api PATCH /users/{id} {status}
```
#### Format 2: Custom Name Syntax

```text
// @api Request Name | METHOD /path {body_fields} ?query_params
// @api Register User | POST /auth/register {name, email, password}
// @api Get All Users | GET /users ?page,limit
// @api Update User Profile | PUT /users/profile {name, phone}
// @api Delete User Account | DELETE /users/{id}
```
#### Format 2: Arrow Syntax

```text
// → METHOD /path {body_fields}
// → POST /auth/login {email, password}
// → GET /users/profile
// → PUT /settings {theme, language}
// → DELETE /sessions/{id}
```

### 📄 Configuration Options

```text
| Field            | Description                           | Default               |
|------------------|---------------------------------------|-----------------------|
| `collectionName` | Name of your Postman collection       | `"My Backend API"`    |
| `baseUrl`        | Base URL for API requests             | `"{{baseUrl}}/api"`   |
| `routesDir`      | Directory containing route files      | `"./routes"`          |
| `folders`        | Map route files to Postman folders    | Auto-generated        |
|----------------------------------------------------------------------------------|
```

### ❓ Troubleshooting Guide

#### Common Issues and Solutions
##### Command Not Found
##### Problem: backend-postman-sync command not recognized

### Solution: Install globally
```bash
npm install -g backend-postman-sync
```

#### Or use npx
```bash
npx backend-postman-sync --help
```

#### Postman API Key Not Found
#### Problem: "Postman API key not found" error

```text
# Check if environment variable is set
echo $env:POSTMAN_API_KEY  # PowerShell
echo $POSTMAN_API_KEY      # CMD/Linux

# Set it if missing (PowerShell)
$env:POSTMAN_API_KEY="pmac_xxx_your_key"

# Set it if missing (CMD)
set POSTMAN_API_KEY=pmac_xxx_your_key

# Set it if missing (Linux/Mac)
export POSTMAN_API_KEY=pmac_xxx_your_key
```

### 📊 Expected Output Examples

#### Dry Run Output Example
```text
🚀 Backend Postman Sync
📡 Sync your backend APIs to Postman automatically

ℹ️ 🏃‍♂️ Dry Run - Checking what would be synced...
ℹ️ Scanning route files...
ℹ️ Found 1 route files
ℹ️   auth.js: 2 endpoints


📋 What would be synced to Postman:
==================================================

📁 Authentication:
   • Register User [Body: name, email, password, phone, role, schoolId]
   • Login User [Body: email, password]
  
==================================================
📊 Dry Run Summary:
   • Collection: My Api Collection
   • 1 folders
   • 2 endpoints total
   • 2 with request bodies
   • 0 with query parameters

💡 This is a dry run - no changes were made to Postman.
   Run "backend-postman-sync sync" to actually sync.
 ```

### 📊 Expected Output Examples

### sync Output Example
```text
🚀 Backend Postman Sync
📡 Sync your backend APIs to Postman automatically

ℹ️ Starting Postman sync...
ℹ️ Scanning route files...
ℹ️ Found 1 route files
ℹ️   auth.js: 2 endpoints
ℹ️ Building Postman collection...
ℹ️ Syncing with Postman...
✅ Connected to Postman as: Your Name
✅ Updated collection: My Api Collection
✅ Sync completed!

📊 Sync Summary:
   • Collection: A
   • 1 folders
   • 2 endpoints
   • 2 with request bodies
   • 0 with query parameters

📁 Folders:
   • Authentication: 2 endpoints

🌐 Collection URL: https://go.postman.co/collection/your-collection-id
 ```

 ###  Useful Links
 1. npm Package: https://www.npmjs.com/package/backend-postman-sync
 2. Documentation: https://github.com/Rahul8945/backend-postman-sync-docs
 3. Report Issues: https://github.com/Rahul8945/backend-postman-sync-docs/issues
 4. Postman API Keys: https://web.postman.co/settings/me/api-keys
 5. Postman Documentation: https://learning.postman.com/docs/developer/echo-api/

## 📄 License

#### MIT License - See LICENSE file for details.
#### Copyright (c) 2024 Rahul Gupta

## Maintained with ❤️ by Rahul Gupta




