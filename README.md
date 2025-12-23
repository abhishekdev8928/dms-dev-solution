# Role-Based Access Control (RBAC) System

A scalable access control system for managing departments, folders, and users with strict permission boundaries.

---

## Overview

### Core Objectives

- **Prevent unauthorized access** – Strict role boundaries prevent privilege escalation
- **Avoid accidental deletion** – Delete permissions limited to responsible roles only
- **Maintain accountability** – Clear ownership trail for all resources
- **Scale efficiently** – Hierarchical structure grows with your organization

---

## Access Architecture

The system implements a two-layer permission model:

**Layer 1: Organizational Level**
- Super Admin – System owner
- Admin – Multi-department manager
- Department Head – Single department owner

**Layer 2: Folder Level**
- Folder Manager – Folder owner
- Folder User – Folder collaborator

---

## Role Definitions

### Super Admin
**Purpose:** Ultimate system authority with full organizational control  
**Scope:** Entire organization

**Permissions:**
- Full CRUD access across all resources
- Create and manage Admin accounts
- Assign/revoke department access for Admins
- Override any permission at any level
- View complete audit trails

**Key Restriction:** None – Super Admin has unrestricted access

---

### Admin
**Purpose:** Manage operations across multiple assigned departments  
**Scope:** Departments explicitly assigned by Super Admin

**Permissions:**
- View all content in assigned departments
- Create folders and upload files
- Delete resources in assigned departments
- Share folders with users
- Manage folder-level permissions within scope

**Key Restrictions:**
- Cannot assign department access (reserved for Super Admin)
- Cannot access unassigned departments
- Cannot modify own department assignments

---

### Department Head
**Purpose:** Complete control over a single department  
**Scope:** One specific department

**Permissions:**
- View all department resources
- Create folders and upload files
- Delete department content
- Share folders within department
- Manage all folders in their department

**Key Restrictions:**
- Cannot access other departments
- Cannot assign department access
- Cannot promote users to Admin level

---

### Folder Manager
**Purpose:** Own and control specific folder(s)  
**Scope:** Assigned folder(s) only

**Permissions:**
- Full control over assigned folder content
- Create and upload files
- Delete folder content
- Share folder publicly or with specific users
- Grant "Folder User" access to others
- Revoke folder access

**Key Restrictions:**
- Cannot access other folders without permission
- Cannot manage departments
- Cannot modify department structure

---

### Folder User
**Purpose:** Safe collaboration within assigned folders  
**Scope:** Folders explicitly shared with them

**Permissions:**
- View folder content
- Create files (if manager allows)
- Upload files (if manager allows)

**Key Restrictions:**
- Cannot delete any content
- Cannot share or grant access
- Cannot modify permissions
- Cannot see folder structure outside their access

---

## Permission Matrix

| **Role** | **Department Assignment** | **View** | **Create/Upload** | **Delete** | **Share** | **Access Scope** |
|----------|---------------------------|----------|-------------------|------------|-----------|------------------|
| **Super Admin** | ✅ Assign to any Admin | ✅ All | ✅ All | ✅ All | ✅ All | Entire System |
| **Admin** | ❌ Cannot assign | ✅ Assigned depts | ✅ Assigned depts | ✅ Assigned depts | ✅ Assigned depts | Assigned Departments |
| **Department Head** | ❌ Cannot assign | ✅ Own dept | ✅ Own dept | ✅ Own dept | ✅ Own dept | Single Department |
| **Folder Manager** | ❌ Cannot assign | ✅ Assigned folders | ✅ Assigned folders | ✅ Assigned folders | ✅ Assigned folders | Assigned Folder(s) |
| **Folder User** | ❌ Cannot assign | ✅ Shared folders | ✅ If allowed | ❌ Never | ❌ Never | Shared Folders Only |

### Core Operations

| Operation | Super Admin | Admin | Dept Head | Folder Manager | Folder User |
|-----------|-------------|-------|-----------|----------------|-------------|
| Create Department | ✅ | ❌ | ❌ | ❌ | ❌ |
| Assign Department Access | ✅ | ❌ | ❌ | ❌ | ❌ |
| Create Folder | ✅ | ✅* | ✅* | ✅** | ❌ |
| Delete Folder | ✅ | ✅* | ✅* | ✅** | ❌ |
| Upload Files | ✅ | ✅* | ✅* | ✅** | ✅*** |
| Delete Files | ✅ | ✅* | ✅* | ✅** | ❌ |
| Share Folder | ✅ | ✅* | ✅* | ✅** | ❌ |

*Within assigned departments only  
**Within assigned folders only  
***If explicitly allowed by Folder Manager

---

## Sharing Model

**Sharing means giving access to a specific folder or files to other users.**

### When to Share

**Share when:**
- Someone needs to see your folder
- A colleague needs to work on your files
- You want to collaborate with specific people

**Don't share when:**
- Users already have access through their role (Admin, Dept Head, etc.)
- It's within the same department and users can already see it

---

## Permission Inheritance for Nested Folders

This system uses **automatic permission inheritance** for nested folders.

### Core Principle
**Permissions flow DOWN the folder tree automatically.**

```
Department: Marketing
  └── Campaign 2025 (Manager: Abhishek)
       ├── Design Assets (automatically managed by Abhishek)
       │    └── Logos (automatically managed by Abhishek)
       └── Reports (automatically managed by Abhishek)
```

### Inheritance Rules

**Rule 1: Folder Manager Inheritance**
- Folder Manager of parent folder = Manager of ALL subfolders
- Full CRUD access applies to entire folder tree
- Can create/delete/share anything within the hierarchy
- No need to assign permissions to each subfolder

**Rule 2: Folder User Inheritance**
- Shared folder access includes ALL subfolders and files
- Permission level (view/edit) applies to entire tree
- User sees complete folder structure

**Rule 3: Folder Manager Assignment Authority**
- Department Head can assign Folder Managers in their department
- Admin can assign Folder Managers in assigned departments
- Super Admin can assign anywhere
- Folder Managers CANNOT assign other Folder Managers

**Rule 4: Subfolder Creation**
- Anyone who creates a subfolder automatically manages it
- Permissions automatically align with parent folder

### Why Inheritance Model?

**Advantages:**
- Simple and intuitive
- Matches real-world expectations
- Easy to implement and scale
- Minimal permission management overhead

**Trade-off:**
- Cannot have different managers for subfolders within same tree
- All-or-nothing access to folder contents

---

## Security Benefits

- **Prevents Privilege Escalation** – Users cannot self-assign elevated permissions
- **Eliminates Accidental Data Loss** – Delete operations restricted to manager-level roles
- **Enforces Clear Accountability** – Every resource has an identifiable owner
- **Scales Cleanly** – Hierarchical structure grows naturally
- **Maintains Separation of Duties** – Department access control separated from operations
- **Supports Audit Compliance** – Complete permission trail for regulatory compliance
