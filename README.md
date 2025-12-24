# DMS Requirements & Current Architecture (Problem Statement Included)

## 1. Purpose of this Document

This document explains:
- The **original requirements** captured in handwritten notes
- The **current implemented parent–child folder structure**
- The **role & drive concepts**
- The **core problem** faced after introducing Personal Drive & Sharing

This README is meant to help **any developer or architect quickly understand**:
- What already exists
- What was added later
- Why the current design is struggling
- What needs to be unified

---

## 2. Drive Requirements (From Initial Design)

The system supports **two main drives**:

1. **Organization Drive**
2. **Personal Drive**

Additionally:
- A **Shared page** exists to show shared files/folders

---

## 3. Organization Drive – Requirements

### 3.1 Department-Based Structure

- Organization Drive is divided into **Departments**
- Each department contains:
  - Folders
  - Subfolders
  - Files

Example:
```
Organization Drive
 ├── Finance
 │    └── Folders → Files
 ├── Marketing
 │    └── Folders → Subfolders → Files
 └── Development
```

---

## 4. Roles & Access Control

> Only top-level roles can assign access.

### 4.1 Roles

| Role | Description |
|----|----|
| Super Admin | Can access everything |
| Admin | Can access assigned departments |
| Department Owner | Owner of a single department |
| Folder Manager | Full access inside assigned folder |
| Folder User | Can only upload/view assigned folders |

---

## 5. Sharing Requirement

- Super Admin (or authorized roles) can:
  - Share files/folders across departments
  - Share with specific users
- Shared items must appear in a **separate “Shared” page**
- Sharing does **not duplicate files**

---

## 6. Current Implemented Data Structure (IMPORTANT)

The DMS is already implemented using a **strict parent–child hierarchy**.

### 6.1 Parent–Child Relationship (Existing Code)

Each entity stores:
- `parent_id`
- `department_id`

### 6.2 Current Hierarchy

| Entity | parent_id | department_id |
|------|----------|---------------|
| Department | NULL | department_id |
| Folder | department_id | department_id |
| Subfolder | folder_id | department_id |
| File | subfolder_id | department_id |

### 6.3 Key Characteristics

- Every child **knows its parent**
- Every folder/file **must have a department_id**
- This design works perfectly for **Organization Drive**
- The hierarchy is already stable and in production

---

## 7. Personal Drive – New Requirement

Personal Drive behaves like Google Drive:

- Completely personal & private
- Files/folders belong to a **user**
- No department ownership
- Users can:
  - Create folders
  - Upload files
  - Share with other users
- Shared personal items appear in **Shared page**

---

## 8. The Core Problem (Very Important)

### 8.1 Why Current Code Breaks

The **existing implementation assumes**:
- Every folder/file belongs to a department
- Every child node has a `department_id`

But **Personal Drive introduces folders/files that**:
- Do NOT belong to any department
- Belong to a **user**
- Still need the same parent–child behavior

This creates a conflict:
> The existing schema is department-centric,  
> but Personal Drive is user-centric.

---

## 9. The Real Challenge

> How to support:
- Organization Drive
- Personal Drive
- Shared files

### WITHOUT:
- Duplicating folder/file tables
- Creating separate APIs
- Breaking existing parent–child logic

### WHILE:
- Reusing the same hierarchy
- Keeping a single folder/file API
- Maintaining strict permission rules

---

## 10. Current Situation Summary

- Folder/file hierarchy already exists
- Parent–child relationships are solid
- Department-based ownership is deeply embedded
- Personal Drive & Sharing were added later
- The system now needs a **unified ownership model**

---

## 11. Problem Statement (In One Line)

> The existing DMS is built around **department-based parent–child hierarchy**,  
> but new requirements (Personal Drive & Sharing) demand a **user-based ownership model**,  
> and the challenge is to **merge both under a single unified API and schema**.

---

## 12. Why This Document Matters

This README exists to:
- Prevent misunderstanding of legacy code
- Explain why refactoring is required
- Align all developers on the same mental model
- Serve as a base for future architecture decisions
