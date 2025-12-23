# Role-Based Access Control (RBAC) – Folder & Department System

This project implements a **strict, scalable, and secure RBAC system** for managing
Departments, Folders, and Users with clear ownership and permission boundaries.

The goal is to:
- Prevent unauthorized access
- Avoid accidental data deletion
- Maintain accountability
- Scale easily across departments and users

---

## 🏗️ Access Levels Overview

The system is divided into **two permission layers**:

1. **Organizational Level**
   - Super Admin
   - Admin
   - Department Head

2. **Folder Level**
   - Folder Manager
   - Folder User

Permissions always flow **top → down**.

---

## 👑 Roles & Responsibilities

### 1️⃣ Super Admin (Highest Authority)

**Purpose:** Controls organization structure

**Permissions:**
- Full system access
- Create Admins
- Assign one or more **Departments to Admins**
- View, Create, Delete, Share anything
- Override all permissions

**Restrictions:**
- None

> Only Super Admin can assign or modify department access.

---

### 2️⃣ Admin

**Purpose:** Manage operations inside assigned departments

**Permissions:**
- Access **only departments assigned by Super Admin**
- View / Create / Upload
- Delete
- Share
- Manage folders inside assigned departments

**Restrictions:**
- ❌ Cannot assign departments
- ❌ Cannot access unassigned departments

---

### 3️⃣ Department Head

**Purpose:** Full control of a single department

**Permissions:**
- View everything inside their department
- Create / Upload
- Delete
- Share

**Restrictions:**
- ❌ Cannot access other departments
- ❌ Cannot assign departments

---

### 4️⃣ Folder Manager

**Purpose:** Own and manage a specific folder

**Permissions:**
- View all content in assigned folder
- Create / Upload
- Delete
- Share folder with:
  - Public
  - Specific users
- Assign **Folder User** access

**Restrictions:**
- ❌ Cannot manage departments
- ❌ Cannot access other folders

> Folder Manager is the **only role allowed to share folder access**.

---

### 5️⃣ Folder User

**Purpose:** Safely work inside assigned folders

**Permissions:**
- View content
- Create / Upload (if allowed)

**Restrictions:**
- ❌ Cannot delete
- ❌ Cannot share
- ❌ Cannot change permissions

---

## 🔐 Permission Matrix

| Role | Assign Dept | View | Create | Delete | Share | Scope |
|----|----|----|----|----|----|----|
| Super Admin | ✅ | ✅ | ✅ | ✅ | ✅ | All |
| Admin | ❌ | ✅ | ✅ | ✅ | ✅ | Assigned Depts |
| Dept Head | ❌ | ✅ | ✅ | ✅ | ✅ | Own Dept |
| Folder Manager | ❌ | ✅ | ✅ | ✅ | ✅ | Assigned Folder |
| Folder User | ❌ | ✅ | ✅ | ❌ | ❌ | Assigned Folder |

---

## 🔄 Sharing Logic

Folders can be shared in two ways:
- **Public**
- **Specific Users**

Shared folders appear under **"Shared With Me"** for users.

Ownership never changes during sharing.

---

## 🛡️ Why This Approach Is Used

### ✅ Prevents privilege escalation
- Admins cannot assign themselves new departments
- Users cannot grant access to others

### ✅ Prevents accidental data loss
- Delete is restricted to responsible roles only

### ✅ Clear ownership & accountability
- Each folder has a single manager
- Sharing actions are traceable

### ✅ Scales cleanly
- Super Admin manages structure
- Operations flow downward
- No permission chaos as users grow

---

## 🔁 Access Flow Diagram (Conceptual)

