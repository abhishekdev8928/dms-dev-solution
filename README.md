# 🔐 Role-Based Access Control (RBAC) System

A **scalable, secure, and enterprise-grade** access control system for managing departments, folders, and users with strict permission boundaries and clear accountability.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Access Architecture](#-access-architecture)
- [Role Definitions](#-role-definitions)
- [Permission Matrix](#-permission-matrix)
- [Sharing Model](#-sharing-model)
- [Security Benefits](#-security-benefits)
- [Access Flow Diagram](#-access-flow-diagram)

---

## 🎯 Overview

### Core Objectives

**Prevent unauthorized access** – Strict role boundaries prevent privilege escalation

**Avoid accidental deletion** – Delete permissions limited to responsible roles only

**Maintain accountability** – Clear ownership trail for all resources

**Scale efficiently** – Hierarchical structure grows cleanly with your organization

---

## 🏗️ Access Architecture

The system implements a **two-layer permission model**:

### Layer 1: Organizational Level
Controls department structure and high-level operations
- **Super Admin** – System owner
- **Admin** – Multi-department manager
- **Department Head** – Single department owner

### Layer 2: Folder Level
Controls granular resource access within departments
- **Folder Manager** – Folder owner
- **Folder User** – Folder collaborator

> **Permission Flow:** Always cascades **top → down** through the hierarchy

---

## 👥 Role Definitions

### 1️⃣ Super Admin

<table>
<tr><td><strong>Purpose</strong></td><td>Ultimate system authority with full organizational control</td></tr>
<tr><td><strong>Scope</strong></td><td>Entire organization</td></tr>
</table>

**✅ Permissions**
- Full CRUD access across all resources
- Create and manage Admin accounts
- Assign/revoke department access for Admins
- Override any permission at any level
- View complete audit trails

**❌ Restrictions**
- None – Super Admin has unrestricted access

> 🔑 **Critical:** Only Super Admin can assign department access to other users

---

### 2️⃣ Admin

<table>
<tr><td><strong>Purpose</strong></td><td>Manage operations across multiple assigned departments</td></tr>
<tr><td><strong>Scope</strong></td><td>Departments explicitly assigned by Super Admin</td></tr>
</table>

**✅ Permissions**
- View all content in assigned departments
- Create folders and upload files
- Delete resources in assigned departments
- Share folders with users
- Manage folder-level permissions within scope

**❌ Restrictions**
- Cannot assign department access (reserved for Super Admin)
- Cannot access unassigned departments
- Cannot modify own department assignments

---

### 3️⃣ Department Head

<table>
<tr><td><strong>Purpose</strong></td><td>Complete control over a single department</td></tr>
<tr><td><strong>Scope</strong></td><td>One specific department</td></tr>
</table>

**✅ Permissions**
- View all department resources
- Create folders and upload files
- Delete department content
- Share folders within department
- Manage all folders in their department

**❌ Restrictions**
- Cannot access other departments
- Cannot assign department access
- Cannot promote users to Admin level

---

### 4️⃣ Folder Manager

<table>
<tr><td><strong>Purpose</strong></td><td>Own and control specific folder(s)</td></tr>
<tr><td><strong>Scope</strong></td><td>Assigned folder(s) only</td></tr>
</table>

**✅ Permissions**
- Full control over assigned folder content
- Create and upload files
- Delete folder content
- Share folder publicly or with specific users
- Grant "Folder User" access to others
- Revoke folder access

**❌ Restrictions**
- Cannot access other folders without permission
- Cannot manage departments
- Cannot modify department structure

> 📢 **Note:** Folder Manager is the **exclusive role** authorized to share folder access

---

### 5️⃣ Folder User

<table>
<tr><td><strong>Purpose</strong></td><td>Safe collaboration within assigned folders</td></tr>
<tr><td><strong>Scope</strong></td><td>Folders explicitly shared with them</td></tr>
</table>

**✅ Permissions**
- View folder content
- Create files (if manager allows)
- Upload files (if manager allows)

**❌ Restrictions**
- Cannot delete any content
- Cannot share or grant access
- Cannot modify permissions
- Cannot see folder structure outside their access

---

## 📊 Permission Matrix

### Comprehensive Access Table

| **Role** | **Department Assignment** | **View** | **Create/Upload** | **Delete** | **Share** | **Access Scope** |
|----------|---------------------------|----------|-------------------|------------|-----------|------------------|
| **Super Admin** | ✅ Assign to any Admin | ✅ All | ✅ All | ✅ All | ✅ All | 🌐 **Entire System** |
| **Admin** | ❌ Cannot assign | ✅ Assigned depts | ✅ Assigned depts | ✅ Assigned depts | ✅ Assigned depts | 🏢 **Assigned Departments** |
| **Department Head** | ❌ Cannot assign | ✅ Own dept | ✅ Own dept | ✅ Own dept | ✅ Own dept | 🏬 **Single Department** |
| **Folder Manager** | ❌ Cannot assign | ✅ Assigned folders | ✅ Assigned folders | ✅ Assigned folders | ✅ Assigned folders | 📁 **Assigned Folder(s)** |
| **Folder User** | ❌ Cannot assign | ✅ Shared folders | ✅ If allowed | ❌ Never | ❌ Never | 📄 **Shared Folders Only** |

### Detailed Permission Breakdown

#### Core Operations

| Operation | Super Admin | Admin | Dept Head | Folder Manager | Folder User |
|-----------|-------------|-------|-----------|----------------|-------------|
| Create Department | ✅ | ❌ | ❌ | ❌ | ❌ |
| Delete Department | ✅ | ❌ | ❌ | ❌ | ❌ |
| Assign Department Access | ✅ | ❌ | ❌ | ❌ | ❌ |
| Create Folder | ✅ | ✅* | ✅* | ✅** | ❌ |
| Delete Folder | ✅ | ✅* | ✅* | ✅** | ❌ |
| Upload Files | ✅ | ✅* | ✅* | ✅** | ✅*** |
| Delete Files | ✅ | ✅* | ✅* | ✅** | ❌ |
| Share Folder | ✅ | ✅* | ✅* | ✅** | ❌ |
| Grant Folder Access | ✅ | ✅* | ✅* | ✅** | ❌ |

**Legend:**
- `*` = Within assigned departments only
- `**` = Within assigned folders only
- `***` = If explicitly allowed by Folder Manager

#### User Management

| Action | Super Admin | Admin | Dept Head | Folder Manager | Folder User |
|--------|-------------|-------|-----------|----------------|-------------|
| Create Admin | ✅ | ❌ | ❌ | ❌ | ❌ |
| Create Dept Head | ✅ | ✅* | ❌ | ❌ | ❌ |
| Create Folder Manager | ✅ | ✅* | ✅* | ❌ | ❌ |
| Create Folder User | ✅ | ✅* | ✅* | ✅** | ❌ |
| Modify User Roles | ✅ | ✅* | ✅* | ✅** | ❌ |

#### Access Control

| Control | Super Admin | Admin | Dept Head | Folder Manager | Folder User |
|---------|-------------|-------|-----------|----------------|-------------|
| View Audit Logs | ✅ All | ✅ Dept scope | ✅ Dept scope | ✅ Folder scope | ❌ |
| Revoke Access | ✅ Any | ✅ Dept scope | ✅ Dept scope | ✅ Folder scope | ❌ |
| Override Permissions | ✅ | ❌ | ❌ | ❌ | ❌ |

---

## 🔄 Sharing Model

### Sharing Mechanisms

Folders can be shared through **two methods**:

#### 1. Public Sharing
- Folder becomes accessible to anyone in the organization
- Users maintain read-only access unless elevated
- Ideal for company-wide resources

#### 2. Specific User Sharing
- Folder shared with named individuals
- Granular access control
- Shared folders appear in user's **"Shared With Me"** section

### Sharing Rules

✅ **Allowed**
- Folder Manager can share their folders
- Admin/Dept Head can share within their scope
- Super Admin can share anything

❌ **Restricted**
- Folder User cannot share
- Sharing never transfers ownership
- Original creator remains the owner
- Share recipients cannot re-share (unless promoted)

### Shared Folder Behavior

- Appears under **"Shared With Me"** for recipients
- Original location structure remains intact
- Permissions determined by recipient's role
- Ownership never changes through sharing

---

## 🛡️ Security Benefits

### ✅ Prevents Privilege Escalation
Admins cannot self-assign new departments, and users cannot grant elevated permissions to themselves or others

### ✅ Eliminates Accidental Data Loss
Delete operations restricted to manager-level roles and above, preventing unintended content removal

### ✅ Enforces Clear Accountability
Every resource has an identifiable owner, and all permission changes are traceable through audit logs

### ✅ Scales Cleanly
Hierarchical structure grows naturally as the organization expands without creating permission chaos or management overhead

### ✅ Maintains Separation of Duties
Department access control (Super Admin) is separated from operational permissions (all other roles)

### ✅ Supports Audit Compliance
Complete permission trail enables regulatory compliance and security audits

---

## 🔁 Access Flow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                        SUPER ADMIN                          │
│                    (System Authority)                       │
│  • Assigns departments to Admins                           │
│  • Full system access                                      │
└────────────────┬───────────────────────────────────────────┘
                 │
                 ├──────────────┬──────────────┐
                 ▼              ▼              ▼
         ┌──────────────┐ ┌──────────────┐ ┌──────────────┐
         │    ADMIN     │ │    ADMIN     │ │    ADMIN     │
         │   (Dept A,B) │ │   (Dept C)   │ │   (Dept D,E) │
         └──────┬───────┘ └──────┬───────┘ └──────┬───────┘
                │                │                │
       ┌────────┴────────┐      │       ┌────────┴────────┐
       ▼                 ▼      ▼       ▼                 ▼
  Department A     Department B │  Department D     Department E
       │                 │       │       │                 │
       ▼                 ▼       ▼       ▼                 ▼
┌─────────────┐   ┌─────────────┐   ┌─────────────┐
│  DEPT HEAD  │   │  DEPT HEAD  │   │  DEPT HEAD  │
│   (Dept C)  │   │             │   │             │
└──────┬──────┘   └─────────────┘   └─────────────┘
       │
       └──────┬──────────┬──────────┬─────────
              ▼          ▼          ▼
        ┌──────────┐ ┌──────────┐ ┌──────────┐
        │  FOLDER  │ │  FOLDER  │ │  FOLDER  │
        │ MANAGER  │ │ MANAGER  │ │ MANAGER  │
        └────┬─────┘ └────┬─────┘ └────┬─────┘
             │            │            │
        ┌────┴────┐  ┌────┴────┐  ┌────┴────┐
        ▼         ▼  ▼         ▼  ▼         ▼
    ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐
    │ FOLDER │ │ FOLDER │ │ FOLDER │ │ FOLDER │
    │  USER  │ │  USER  │ │  USER  │ │  USER  │
    └────────┘ └────────┘ └────────┘ └────────┘
```

### Flow Explanation

1. **Super Admin** controls the organizational structure
2. **Admins** receive department assignments from Super Admin
3. **Department Heads** manage single departments
4. **Folder Managers** control specific folders within departments
5. **Folder Users** access shared folders for collaboration

> **Permission Inheritance:** Higher roles automatically include all permissions of lower roles within their scope

---

## 📝 Implementation Notes

### Best Practices

1. **Assign Minimal Necessary Permissions** – Start restrictive, elevate as needed
2. **Regular Access Reviews** – Audit user permissions quarterly
3. **Document Department Assignments** – Maintain clear records of Admin scope
4. **Use Groups for Folder Sharing** – Scale sharing through user groups
5. **Monitor Folder Manager Actions** – Track sharing and permission changes

### Common Scenarios

**Scenario 1:** New employee needs access to Marketing folders
- Department Head or Folder Manager shares specific folders
- Employee receives Folder User role
- No delete or share permissions granted

**Scenario 2:** Employee promoted to team lead
- Super Admin or Admin promotes to Folder Manager
- User can now manage their team's folders
- Can share with team members as needed

**Scenario 3:** Cross-department collaboration
- Admin with both dept assignments can bridge access
- Or Folder Manager shares specific folders publicly
- Maintains clear ownership boundaries

---

## 🚀 Getting Started

1. **Super Admin** creates organizational structure (departments)
2. **Super Admin** assigns Admins to departments
3. **Admins/Dept Heads** create folders and assign Folder Managers
4. **Folder Managers** share folders and grant access to Folder Users
5. **All users** operate within their permission boundaries

---

## 📞 Support

For questions about role assignments or permission issues, contact your **Super Admin** or system administrator.

---

**Version:** 2.0  
**Last Updated:** December 2025  
**Security Classification:** Internal Use

