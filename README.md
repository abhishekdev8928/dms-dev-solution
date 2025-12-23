# RBAC System - Implementation Guide

## 🎯 Purpose
This document provides a complete implementation guide for building a Role-Based Access Control (RBAC) system for your Document Management System.

---

## 📋 Table of Contents
1. [System Overview](#overview)
2. [The 5 Roles Explained](#roles)
3. [Access Control Rules](#access-rules)
4. [Implementation Steps](#implementation)
5. [Testing Checklist](#testing)
6. [Common Mistakes to Avoid](#pitfalls)

---

## 🏗️ System Overview {#overview}

### What We're Building
A hierarchical file management system where access is controlled by user roles and folder assignments.

### System Hierarchy
```
Department: Marketing
  │
  ├── Folder: Campaign 2025
  │     ├── Subfolder: Design Assets
  │     │     ├── Subfolder: Logos
  │     │     │     └── File: final-logo.png
  │     │     └── File: banner.jpg
  │     └── File: budget.xlsx
  │
  └── Folder: Social Media
        ├── Subfolder: Instagram Posts
        │     └── File: post-jan.png
        └── File: calendar.xlsx

Department: Sales
  │
  └── Folder: Q4 Reports
        ├── Subfolder: January
        │     └── File: jan-report.pdf
        └── File: summary.xlsx
```

### Key Concepts
- **Department**: Top-level organizational unit (Marketing, Sales, HR, Finance)
- **Folder**: Container to organize files - can have unlimited nesting levels
- **File**: The actual document/image/video stored in folders
- **Role**: Defines what actions a user can perform system-wide
- **Folder Assignment**: Determines which folders a user can access

---

## 👥 The 5 Roles Explained {#roles}

### 🔴 Role 1: Super Admin
**Who**: System owner, CTO, IT Head

**Powers**:
- ✅ Create/delete departments
- ✅ Access ALL departments and folders
- ✅ Create/delete folders anywhere
- ✅ Upload/delete files anywhere
- ✅ Assign Admins to departments
- ✅ Assign Department Heads
- ✅ Assign Folder Managers

**Restrictions**:
- ❌ None - complete system access

**Example**:
```
Rajesh (Super Admin):
- Creates "Marketing" department ✅
- Creates "Sales" department ✅
- Makes Priya an Admin ✅
- Assigns Priya to Marketing + Sales ✅
- Can access ANY file in ANY department ✅
```

---

### 🟠 Role 2: Admin
**Who**: Senior manager handling multiple departments

**Powers**:
- ✅ Access assigned departments (multiple allowed)
- ✅ Create/delete folders in assigned departments
- ✅ Upload/delete files in assigned departments
- ✅ Assign Folder Managers in assigned departments
- ✅ View all folders/files in assigned departments

**Restrictions**:
- ❌ Cannot create new departments
- ❌ Cannot access non-assigned departments
- ❌ Cannot assign other Admins (only Super Admin can)
- ❌ Cannot assign Department Heads

**Example**:
```
Priya (Admin assigned to Marketing + Sales):
- Can create folders in Marketing ✅
- Can upload files to Sales ✅
- Can assign Rahul as Folder Manager in Marketing ✅
- Cannot access HR department ❌ (not assigned)
- Cannot create new departments ❌
- Cannot make another user an Admin ❌
```

---

### 🟡 Role 3: Department Head
**Who**: Head/Manager of ONE specific department

**Powers**:
- ✅ Full access to their assigned department
- ✅ Create/delete folders in their department
- ✅ Upload/delete files in their department
- ✅ Assign Folder Managers in their department
- ✅ View all folders/files in their department

**Restrictions**:
- ❌ Can only manage ONE department
- ❌ Cannot access other departments
- ❌ Cannot create new departments
- ❌ Cannot assign Admins or other Department Heads

**Example**:
```
Sneha (Department Head of Marketing):
- Can create any folder in Marketing ✅
- Can delete any file in Marketing ✅
- Can assign Rahul as Folder Manager ✅
- Cannot see Sales department ❌
- Cannot access HR department ❌
- Cannot make anyone an Admin ❌
```

---

### 🟢 Role 4: Folder Manager
**Who**: Owner/Manager of specific folder(s)

**Powers**:
- ✅ Full access to assigned folders
- ✅ Create/delete files in assigned folders
- ✅ Create subfolders inside assigned folders
- ✅ Delete their assigned folders
- ✅ View all content in assigned folders (including nested subfolders)

**Restrictions**:
- ❌ Cannot access folders they don't manage
- ❌ Cannot create root-level folders in department
- ❌ Cannot assign other Folder Managers
- ❌ Cannot access department level

**Example**:
```
Rahul (Folder Manager of "Campaign 2025"):
- Can upload files to "Campaign 2025" ✅
- Can create "Design Assets" subfolder ✅
- Can delete any file in "Campaign 2025" ✅
- Can delete "Campaign 2025" folder itself ✅
- Cannot access "Social Media" folder ❌
- Cannot create new root folder in Marketing ❌
```

**Important**: Folder Manager automatically gets access to ALL subfolders inside their assigned folder.

---

### 🔵 Role 5: Folder User
**Who**: Team member who needs basic access

**Powers**:
- ✅ View folders they have access to
- ✅ View files in accessible folders
- ✅ Upload files (basic contribution)

**Restrictions**:
- ❌ Cannot delete any files or folders
- ❌ Cannot create new folders
- ❌ Cannot manage other users
- ❌ Very limited access - read and upload only

**Example**:
```
Anjali (Folder User assigned to "Campaign 2025"):
- Can view files in "Campaign 2025" ✅
- Can upload her work files ✅
- Cannot delete anything ❌
- Cannot create subfolders ❌
- Cannot give access to others ❌
```

**Note**: Folder User is the most restricted role - designed for basic collaboration only.

---

## 🔐 Access Control Rules {#access-rules}

### Access Resolution Priority
When checking if a user can perform an action, check in this order:

```
1. Super Admin          → ALLOW ALL (bypass everything)
2. Department Access    → Check if user has department access
3. Folder Assignment    → Check if user is assigned to folder
4. Role Permissions     → Check what role allows
5. Default              → DENY
```

### Decision Tree
```
┌─────────────────────┐
│ Is Super Admin?     │──YES──> ALLOW ALL
└──────────┬──────────┘
           NO
           ↓
┌─────────────────────┐
│ Admin/Dept Head     │──YES──> Check department access
│ accessing dept?     │
└──────────┬──────────┘
           NO
           ↓
┌─────────────────────┐
│ Is assigned to      │──YES──> Check role permissions
│ this folder?        │
└──────────┬──────────┘
           NO
           ↓
         DENY
```

### Role Permission Matrix

| Role | View | Upload | Delete | Create Folder | Manage Users |
|------|------|--------|--------|---------------|--------------|
| Super Admin | ✅ All | ✅ All | ✅ All | ✅ All | ✅ All |
| Admin | ✅ Dept | ✅ Dept | ✅ Dept | ✅ Dept | ✅ Folder Mgr |
| Dept Head | ✅ Dept | ✅ Dept | ✅ Dept | ✅ Dept | ✅ Folder Mgr |
| Folder Manager | ✅ Folder | ✅ Folder | ✅ Folder | ✅ Subfolder | ❌ |
| Folder User | ✅ Folder | ✅ Folder | ❌ | ❌ | ❌ |

---

## 🛠️ Implementation Steps {#implementation}

### Step 1: Create Central Access Function

```javascript
/**
 * Single source of truth for all access decisions
 * @param {Object} user - Current user object
 * @param {Object} resource - Folder/File being accessed
 * @param {String} action - Action to perform (VIEW, UPLOAD, DELETE, etc.)
 * @returns {Boolean} - true if allowed, false if denied
 */
function canAccess(user, resource, action) {
  // Gate 1: Super Admin bypass
  if (user.role === 'SUPER_ADMIN') {
    return true;
  }
  
  // Gate 2: Department-level access (Admin/Dept Head)
  if (user.role === 'ADMIN' || user.role === 'DEPT_HEAD') {
    if (!userHasDepartmentAccess(user, resource.departmentId)) {
      return false;
    }
    return roleAllows(user.role, action);
  }
  
  // Gate 3: Folder-level access (Folder Manager/User)
  if (user.role === 'FOLDER_MANAGER' || user.role === 'FOLDER_USER') {
    if (!userHasFolderAccess(user, resource.folderId || resource.id)) {
      return false;
    }
    return roleAllows(user.role, action);
  }
  
  // Default: Deny
  return false;
}
```

### Step 2: Implement Support Functions

```javascript
// Define what each role can do
const ROLE_PERMISSIONS = {
  SUPER_ADMIN: ['VIEW', 'UPLOAD', 'DELETE', 'CREATE_FOLDER', 'MANAGE_USERS'],
  ADMIN: ['VIEW', 'UPLOAD', 'DELETE', 'CREATE_FOLDER', 'ASSIGN_FOLDER_MANAGER'],
  DEPT_HEAD: ['VIEW', 'UPLOAD', 'DELETE', 'CREATE_FOLDER', 'ASSIGN_FOLDER_MANAGER'],
  FOLDER_MANAGER: ['VIEW', 'UPLOAD', 'DELETE', 'CREATE_SUBFOLDER'],
  FOLDER_USER: ['VIEW', 'UPLOAD']
};

function roleAllows(role, action) {
  return ROLE_PERMISSIONS[role]?.includes(action) || false;
}

function userHasDepartmentAccess(user, departmentId) {
  // Check if user is assigned to this department
  return user.assignedDepartments?.includes(departmentId) || false;
}

function userHasFolderAccess(user, folderId) {
  // Check if user is assigned to this folder
  if (user.assignedFolders?.includes(folderId)) {
    return true;
  }
  
  // Check if user has access to parent folder
  const folder = getFolderById(folderId);
  if (folder?.parentId) {
    return userHasFolderAccess(user, folder.parentId);
  }
  
  return false;
}
```

### Step 3: Use in All Endpoints

```javascript
// Example: File Upload Endpoint
app.post('/api/folders/:id/upload', async (req, res) => {
  const user = req.user;
  const folder = await getFolderById(req.params.id);
  
  if (!folder) {
    return res.status(404).json({ error: 'Folder not found' });
  }
  
  // Single permission check
  if (!canAccess(user, folder, 'UPLOAD')) {
    return res.status(403).json({ error: 'Access denied' });
  }
  
  // Proceed with upload
  const file = await saveFile(req.file, folder.id, user.id);
  return res.json(file);
});

// Example: Folder Delete Endpoint
app.delete('/api/folders/:id', async (req, res) => {
  const user = req.user;
  const folder = await getFolderById(req.params.id);
  
  if (!folder) {
    return res.status(404).json({ error: 'Folder not found' });
  }
  
  if (!canAccess(user, folder, 'DELETE')) {
    return res.status(403).json({ error: 'Access denied' });
  }
  
  await deleteFolder(folder.id);
  return res.json({ success: true });
});
```

---

## ✅ Testing Checklist {#testing}

### Scenario 1: Super Admin Access
- [ ] Super admin can VIEW any folder/file
- [ ] Super admin can UPLOAD to any folder
- [ ] Super admin can DELETE any folder/file
- [ ] Super admin can CREATE folders anywhere
- [ ] Super admin can assign any role

### Scenario 2: Admin Access
- [ ] Admin can access assigned departments
- [ ] Admin CANNOT access non-assigned departments
- [ ] Admin can create folders in assigned departments
- [ ] Admin can assign Folder Managers
- [ ] Admin CANNOT assign other Admins

### Scenario 3: Department Head Access
- [ ] Dept Head can access their department only
- [ ] Dept Head can create/delete folders in their dept
- [ ] Dept Head can assign Folder Managers
- [ ] Dept Head CANNOT access other departments
- [ ] Dept Head CANNOT create departments

### Scenario 4: Folder Manager Access
- [ ] Folder Manager can access assigned folders
- [ ] Folder Manager can create subfolders
- [ ] Folder Manager can upload/delete files
- [ ] Folder Manager CANNOT access other folders
- [ ] Folder Manager CANNOT create root folders

### Scenario 5: Folder User Access
- [ ] Folder User can view assigned folders
- [ ] Folder User can upload files
- [ ] Folder User CANNOT delete files
- [ ] Folder User CANNOT create folders
- [ ] Folder User CANNOT access other folders

### Scenario 6: Folder Inheritance
- [ ] User with folder access can access all subfolders
- [ ] User without parent folder access cannot access subfolders
- [ ] Folder Manager can manage all nested subfolders
- [ ] Folder User can view all nested content

---

## ⚠️ Common Mistakes to Avoid {#pitfalls}

### ❌ DON'T DO THIS:

**1. Multiple permission checks scattered everywhere**
```javascript
// Bad - duplicate logic
if (user.role === 'ADMIN') { /* allow */ }
// ...in another file
if (user.role === 'ADMIN') { /* allow */ }
```

**2. Checking role without checking access**
```javascript
// Bad - role alone isn't enough
if (user.role === 'FOLDER_MANAGER') {
  return true; // Wrong! Must check folder assignment
}
```

**3. Ignoring folder hierarchy**
```javascript
// Bad - not checking parent folders
if (user.assignedFolders.includes(subFolderId)) {
  return true; // Wrong! Should check parent too
}
```

### ✅ DO THIS INSTEAD:

**1. Single access function**
```javascript
// Good - one place to check
if (!canAccess(user, resource, action)) {
  return res.status(403).json({ error: 'Access denied' });
}
```

**2. Check role AND access**
```javascript
// Good - comprehensive check
if (!userHasFolderAccess(user, folderId)) return false;
if (!roleAllows(user.role, action)) return false;
return true;
```

**3. Recursive folder checking**
```javascript
// Good - checks entire hierarchy
function userHasFolderAccess(user, folderId) {
  if (user.assignedFolders.includes(folderId)) return true;
  const folder = getFolderById(folderId);
  if (folder.parentId) return userHasFolderAccess(user, folder.parentId);
  return false;
}
```

---

## 📊 Golden Rules

1. **Role defines WHAT you can do** (capability ceiling)
2. **Assignment defines WHERE you can do it** (department/folder boundary)
3. **Super Admin bypasses all checks**
4. **ONE function decides ALL access** (`canAccess()`)
5. **Check priority**: Super Admin → Department → Folder → Deny
6. **Folder Manager gets ALL subfolders** (automatic inheritance)
7. **Admin/Dept Head can only access assigned departments**
8. **Folder assignments are explicit** (not automatic)

---

## 🔗 Quick Reference

### Action Types
- `VIEW` - Can see the resource and its metadata
- `UPLOAD` - Can create new files in folder
- `DELETE` - Can remove files or folders
- `CREATE_FOLDER` - Can create new folders
- `CREATE_SUBFOLDER` - Can create folders inside assigned folder
- `MANAGE_USERS` - Can assign roles and permissions
- `ASSIGN_FOLDER_MANAGER` - Can assign Folder Manager role

### Basic Usage
```javascript
// In any endpoint:
if (!canAccess(user, resource, 'VIEW')) {
  return res.status(403).json({ error: 'Access denied' });
}
```

---

## 📝 Implementation Checklist

- [ ] Create `accessControl.js` with `canAccess()` function
- [ ] Define `ROLE_PERMISSIONS` constant
- [ ] Implement `roleAllows()` function
- [ ] Implement `userHasDepartmentAccess()` function
- [ ] Implement `userHasFolderAccess()` function (with recursion)
- [ ] Replace all scattered permission checks with `canAccess()`
- [ ] Add permission checks to all API endpoints
- [ ] Test all 6 core scenarios from testing checklist
- [ ] Add access logging for audit trail
- [ ] Document which endpoints require which permissions

---

## 🆘 When Stuck

Ask these questions:
1. **Is this a Role question?** → Can they do this action at all?
2. **Is this a Department question?** → Are they assigned to this department?
3. **Is this a Folder question?** → Are they assigned to this folder?

Then apply the decision tree from top to bottom.

**Still confused?** Draw it out:
```
User: [Role] [Assigned Depts] [Assigned Folders]
Resource: [Type] [Department] [Folder]
Action: [What they want to do]

→ Run through decision tree step by step
```

---

**END OF GUIDE**
