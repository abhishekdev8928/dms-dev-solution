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

### What is "Sharing"?

**Sharing** means **giving access to a specific folder or files to other users.**

That's it. Simple.

> 💡 **Core Concept:** You use sharing when you want someone else to access your folder/files.

### When Do You Share?

**You share when:**
- Someone needs to see your folder
- A colleague needs to work on your files
- You want to collaborate with specific people
- You need to give temporary access

**You DON'T share when:**
- Users already have access through their role (Admin, Dept Head, etc.)
- It's within the same department and users can already see it

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

## 🤔 Common Questions & Design Decisions

### The Nested Folder Problem

When implementing a folder-based DMS, three critical questions arise:

**Question 1: Folder Manager Scope with Nested Folders**
If a user (e.g., Abhishek) is assigned as a Folder Manager of a top-level folder, does that automatically grant them full access to all nested subfolders and files within that folder? Or do nested folders require separate Folder Manager assignments?

**Question 2: Folder User Visibility in Nested Folders**
When a Folder Manager shares a folder with a Folder User, does the Folder User have access to all subfolders and files inside it, or only the files in the shared top-level folder?

**Question 3: Assigning Folder Manager Role**
Who has the authority to assign a user as a Folder Manager for a folder? Can it be Super Admin only, Admin/Department Head, or an existing Folder Manager themselves?

---

### ✅ Our Solution: Permission Inheritance Model

This system uses **automatic permission inheritance** for nested folders to maintain simplicity and intuitive behavior.

#### Core Principle

**Permissions flow DOWN the folder tree automatically.**

```
Department: Marketing
  └── Campaign 2025 (Manager: Abhishek)
       ├── Design Assets (automatically managed by Abhishek)
       │    └── Logos (automatically managed by Abhishek)
       ├── Reports (automatically managed by Abhishek)
       └── budget.xlsx (automatically accessible)
```

---

### 📋 Inheritance Rules

#### Rule 1: Folder Manager Inheritance
```
✅ Folder Manager of parent folder = Manager of ALL subfolders
✅ Full CRUD access applies to entire folder tree
✅ Can create/delete/share anything within the hierarchy
✅ No need to assign permissions to each subfolder
```

**Example:**
- Abhishek is Folder Manager of "Campaign 2025"
- He automatically manages "Design Assets", "Logos", and "Reports"
- He can delete, share, or modify anything inside

#### Rule 2: Folder User Inheritance
```
✅ Shared folder access includes ALL subfolders and files
✅ Permission level (view/edit) applies to entire tree
✅ User sees complete folder structure
```

**Example:**
- Priya is given Folder User access to "Campaign 2025"
- She can view "Design Assets", "Logos", "Reports", and all files
- Her permission level (view-only or edit) applies everywhere inside

#### Rule 3: Folder Manager Assignment Authority
```
✅ Department Head can assign Folder Managers in their department
✅ Admin can assign Folder Managers in assigned departments
✅ Super Admin can assign anywhere
❌ Folder Managers CANNOT assign other Folder Managers
```

#### Rule 4: Subfolder Creation
```
✅ Anyone who creates a subfolder automatically manages it
✅ Folder Manager creating subfolder = Manager of that subfolder
✅ Permissions automatically align with parent folder
```

---

### 🎯 Why We Chose Inheritance Model

#### ✅ Advantages

**1. Simplicity**
- Users intuitively understand "if I can access a folder, I can access what's inside"
- Matches real-world expectations
- Minimal permission management overhead

**2. Ease of Implementation**
- Permissions stored only at top-level folders
- Simple permission check logic
- Fewer database queries

**3. User Experience**
- No confusion about "which subfolder can I access?"
- Natural folder navigation
- Predictable behavior

**4. Scalability**
- Folder hierarchies can grow without permission complexity
- One permission assignment covers entire tree
- Reduced administrative burden

#### ⚠️ Trade-offs

**Limited Granularity**
- Cannot have different managers for subfolders within same tree
- All-or-nothing access to folder contents

**When This Might Not Work:**
- Complex organizations needing subfolder-level control
- Different teams managing different subfolders of same parent
- Highly compartmentalized security requirements

> **Note:** For 95% of use cases, inheritance provides the right balance of security, usability, and simplicity. Advanced permission models can be added later if specific requirements emerge.

---


    department_id: parent.department_id,  // Inherit department
    owner_id: userId
  };
  
  return createFolder(subfolder);
  // No separate permission needed - inherits from top
}
```

---

### 🔍 Practical Examples

#### Example 1: Deep Nesting
```
Marketing Department
  └── Q1 2025 Campaign (Manager: Abhishek)
       └── Social Media
            └── Instagram
                 └── Stories
                      └── Week 1
                           └── video.mp4
```

**Permission Check for video.mp4:**
1. Traverse up to "Q1 2025 Campaign" (top-level)
2. Check if user has permission on "Q1 2025 Campaign"
3. If yes → Access granted to video.mp4

#### Example 2: Sharing Mid-Level Folder
```
User tries to share "Instagram" folder specifically
```

**System Behavior:**
- Resolves to top-level "Q1 2025 Campaign"
- Shares entire "Q1 2025 Campaign" tree
- Shared user gets access to all siblings and parents too

**Alternative (if needed):**
- System can restrict sharing to only the specific subfolder
- Would require storing permission at "Instagram" level
- Makes "Instagram" effectively a new top-level for permissions

#### Example 3: Cross-Department Sharing
```
Abhishek (Marketing) wants to share "Brand Guidelines" with Sales team
```

**System Behavior:**
1. Abhishek is Folder Manager of "Brand Guidelines" (in Marketing dept)
2. He shares with Priya (Sales dept member)
3. Permission stored: folder="Brand Guidelines", user=Priya, level="user"
4. Priya now sees "Brand Guidelines" in "Shared With Me"
5. She can access all subfolders and files inside

---

## ❓ Additional Design Questions & Our Approach

### Question 4: Can Folder Users Create Subfolders?

**The Question:**
Should a Folder User (someone who has been given access to a folder) be allowed to create subfolders or upload files inside that shared folder?

**Our Approach: NO - Folder Users CANNOT create subfolders**

#### Why This Decision?

**1. Structural Control**
- Creating subfolders = Changing folder organization
- Only managers should control structure
- Prevents chaos from multiple users creating folders

**2. Clear Role Separation**
- Folder Manager = Structure & Organization
- Folder User = Content & Collaboration
- Clean separation prevents confusion

**3. Accountability**
- Manager maintains folder organization
- Users focus on their work
- Clear ownership of folder structure

#### What Folder Users CAN Do

✅ **Allowed:**
- View all files and subfolders
- Upload files to existing subfolders (if granted permission)
- Download files
- Edit files (if granted permission)

❌ **Not Allowed:**
- Create new subfolders
- Delete any content
- Share the folder
- Change folder structure
- Modify permissions

#### The Workflow

**If a Folder User needs a new subfolder:**
1. Request Folder Manager to create it
2. Manager creates the subfolder
3. Manager grants appropriate access
4. User uploads to the new subfolder

This maintains clean organization and accountability.

---

### Summary of Our Design Approach

| Aspect | Our Approach | Reason |
|--------|-------------|--------|
| **Nested Folder Permissions** | Inheritance Model | Simplicity and intuitive behavior |
| **Subfolder Access** | Automatic from parent | User expects to see everything inside |
| **Folder Manager Assignment** | By Admin/Dept Head | Clear authority chain |
| **Folder User Subfolder Creation** | ❌ Not Allowed | Maintains structure control |
| **Folder User File Upload** | ✅ Allowed (if granted) | Enables collaboration |

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
