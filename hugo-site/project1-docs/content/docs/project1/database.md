---
title: "Database Structure"
weight: 2
---

## MySQL / SQLite Database Usage

The project uses four tables:

- **users**
- **revokedTokens**
- **friendships**
- **friendRequests**

---

## Table: users

| Column      | Type | Description |
|-------------|------|-------------|
| id | INTEGER PRIMARY KEY AUTOINCREMENT | Unique user ID |
| username | TEXT UNIQUE NOT NULL | Username |
| password | TEXT NOT NULL | Hashed password |
| role | TEXT DEFAULT 'user' | user/admin |
| createdAt | DATETIME | Creation time |
| updatedAt | DATETIME | Last update |

### APIs using this table
- POST /api/auth/register  
- POST /api/auth/login  
- GET /api/users  
- POST /api/users/me/password  

---

## Table: revokedTokens

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER PRIMARY KEY | Unique ID |
| token | TEXT | JWT token string |
| revokedAt | DATETIME | Time of revocation |

### APIs using this table
- POST /api/auth/logout  
- Middleware token validation  

---

## Table: friendships

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER PRIMARY KEY |
| userId | INT | First user |
| friendId | INT | Second user |
| createdAt | DATETIME | Time created |

### APIs
- GET /api/friends  
- DELETE /api/friends/{id}  

---

## Table: friendRequests

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER PRIMARY KEY |
| senderId | INT | Sender |
| receiverId | INT | Receiver |
| status | TEXT | pending/accepted/rejected |
| createdAt | DATETIME | Timestamp |

### APIs
- POST /api/friendRequests  
- GET /api/friendRequests  
- PUT /api/friendRequests/{id}  
