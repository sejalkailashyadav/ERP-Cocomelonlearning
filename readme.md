#  ERP Children Management System 

###  Overview

This **ERP (Enterprise Resource Planning)** system is designed for managing multiple centers efficiently.
It provides **role-based access** for Super Admins, Admins, and Managers to handle centers, employees, children, classes, fees, reports, and documentation—all from a centralized dashboard.

---

## 👤 Roles & Permissions

### 1. Super Admin

* Has **full control** over the entire ERP system.
* Can **create, edit, and delete** Admins and Managers.
* Manages all centers, employees, classes, and reports.
* Access to **Role & Permission CRUD** for assigning roles (Admin/Manager) to specific centers.

#### 🔧 Role & Permission Module 

**Purpose:** Allow Super Admin to manage user roles and assign them to centers.

**Fields:**

* Role Name (Admin / Manager)
* User Name
* Email
* Password
* Assigned Center
* Permissions (checkboxes: View / Edit / Delete / Approve)
* Status (Active / Inactive)

**Functions:**

* ➕ Create Role
* 📝 Edit Role
* ❌ Delete Role
* 👁️ View Assigned Permissions

---

## 🧩 Core Modules

### 1. Dashboard Management

#### 🧠 Super Admin Dashboard

* Displays overall summary across all centers.
* KPIs: Total Centers, Active Children, Total Fees Collected, Active Managers, Pending Approvals.

#### 🧭 Admin Dashboard

* Displays stats of their **specific center**.
* KPIs: Classes, Active Students, Fees Summary, Monthly Reports.

#### 👩‍💼 Manager Dashboard

* Displays stats for **assigned center only** with **limited access**.

---

### 2. Center Management 

Handles center-level info and legal documentation.

#### Fields:

* Center Name
* Address
* Email
* Phone Number
* License No
* Facility Number
* Licensing Name
* Licensing Email
* Licensing Mobile Number
* Number of Classrooms *(auto-create classrooms when center is created)*

#### Documents Section:

* Business License
* Facility License
* Incorporation Certificate
* CCOF
* Inspection Reports
* Gallery (File Name / View / Replace / Delete)

#### Operations:

* ➕ Create
* 📝 Edit
* ❌ Delete
* 👁️ View Details

---

### 3. Class Management 

#### Purpose: Manage classrooms, capacity, and enrolled children.

#### Fields:

* Class Name
* Amount / Total Children
* Schedule Document
* Status Count (Active / Withdrawn / Review / Archive)
* Gallery 
* Children Overview (Child Name / Parent Name / Days / Status)

#### Operations:

* ➕ Create
* 📝 Edit
* ❌ Delete

---

### 4. Fees Master 

#### Purpose:
Manage and calculate fees per center and class.

#### Fields:

* Center (linked to Center Management)
* Class (linked to Class Management)
* Fees Name
* Monthly Fees
* CCFRI
* Total

#### Operations:

* ➕ Add Fee Plan
* 📝 Edit Fee Plan
* ❌ Delete Fee Plan

---

### 5. Child Master 

#### Workflow:

When a child is enrolled:

1. Select **Center → Class → Fees → Days**
2. Child is created in **Review State** by default
3. Admin/Manager can approve → becomes **Active**
4. When withdrawn → state = **Withdrawn**
5. A **cron job** runs monthly end → converts *Withdrawn → Archived*

#### Fields:

* Child Name
* Parent Name
* Parent Mobile
* Class
* Fees
* Days
* Status
* Documents
* Profile Photo
* View Page (full details view)

#### Status Flow:

`Review ➜ Active ➜ Withdrawn ➜ Archived (Auto by Cron)`

###  Status Logic

| Status        | Description                             | Updated By      | Effect on Reports                |
| ------------- | --------------------------------------- | --------------- | -------------------------------- |
| **Review**    | Default when created, pending approval. | System          | Not shown in reports.            |
| **Active**    | Approved and currently enrolled.        | Admin / Manager | Included in monthly reports.     |
| **Withdrawn** | Child leaves the center.                | Admin / Manager | Included for current month only. |
| **Archived**  | Automatically archived post month-end.  | System          | Excluded from active reports.    |

---

#### Operations:

* ➕ Create Child
* 📝 Edit Child
* ❌ Delete Child
* ✅ Approve Child
* 🔁 Auto Archive (via cron)

---

### 6. Report Master

#### Purpose:

Generate detailed monthly reports per center/class.

#### Fields:

* Select Center
* Select Class
* Select Month
* Child Name
* Parent
* Class
* Fee Plan
* Monthly Fee
* CCFRI
* ACCB
* Total
* Institution Number
* Transit Number
* Account Number
* Manager Notes
* Admin Notes

#### Features:

* Generate Monthly Reports
* List All Reports
* Edit Pre-Created Reports
* Manager View (limited access – hides Admin Notes)

---

### 7. Employee Master 

#### Purpose:
Centralized management of all employees, integrated with notifications and waiting list tracking.

#### Fields:

* Employee Name
* Designation
* Contact Info
* Resume (upload)
* Documents (upload)
* Notes
* Status

#### Features:

* Public Link Generation for each employee (for HR view)
* View, Edit, Delete

---

### 8. Employee Waiting List

#### Source:

* Auto-fetched from **WordPress Website Form** using cron job
* Can also be added manually

#### Fields:

* First Name
* Last Name
* Designation
* Mobile Number
* Email
* Reference
* Resume
* Other Documents
* Notes
* Expected Start Date

#### Operations:

* ➕ Add Employee
* 📝 Edit Employee
* ❌ Delete
* 🔁 Auto Import from WordPress Form (via Cron Job)

---

### 9. Waiting List (for Children)

#### Purpose:

Store inquiries or upcoming admissions.

#### Fields:

* Child Name
* Parent Name
* Contact Start Date
* Created At
* Status
* Care Type
* Center Name
* Notes
* Actions

#### Features:

* Auto Sync from Website Form (via Cron Job)
* Manual Add Option
* CRUD Operations

---

## ⚙️ Manager Role (Limited Access)

Managers can:

* View **assigned centers only**
* Access:

  * Center Details (read-only)
  * Class Management
  * Child Master
  * Report Master (without Admin Notes)

Cannot:

* Add or delete centers
* Access Role & Permission Module
* Modify Fees Master

---

## 🕒 Automation (Cron Jobs)

1. **Withdrawn → Archived Conversion**

   * Runs monthly at end of the month.
   * Moves all *Withdrawn* children to *Archive* state.

2. **Website → ERP Data Sync**

   * Fetches new employee/child form data from WordPress and inserts into ERP Waiting List.

---

## 🧾 Summary Table

| Module                | CRUD | Role Access                 | Automation        |
| --------------------- | ---- | --------------------------- | ----------------- |
| Dashboard             | View | All                         | —                 |
| Center Management     | ✅    | Super Admin, Admin          | —                 |
| Class Management      | ✅    | Super Admin, Admin, Manager | —                 |
| Fees Master           | ✅    | Super Admin, Admin          | —                 |
| Child Master          | ✅    | Super Admin, Admin, Manager | Withdrawn→Archive |
| Report Master         | ✅    | Super Admin, Admin, Manager | —                 |
| Employee Master       | ✅    | Super Admin                 | —                 |
| Employee Waiting List | ✅    | Super Admin, Admin          | Website→ERP Sync  |
| Waiting List          | ✅    | Super Admin, Admin          | Website→ERP Sync  |
| Role & Permission     | ✅    | Super Admin                 | —                 |

---

## 🧠 Tech Stack (for context)

* **Tech Stack:** Node.js, Express, React
* **Database:** MySQL
* **Hosting::** Hostinger Shared Hosting (Canada Server, .ca domain)
* **Storage::** Local (Hostinger Server)
* **Automation:** Cron Jobs (monthly, hourly)

---

## 🔗 Live Demo

You can explore the ERP system online:

**Website:** [https://erp.cocomelonlearning.com/](https://erp.cocomelonlearning.com/)

**Notes:**

* This is a live demo of the ERP system.
* Features visible include:

  * Dashboard Overview
  * Center & Class Management
  * Child Master with Status Workflow
  * Waiting List Management
  * Monthly Reports
  * Notifications
---

<!--
## 📽️ Live Demo (YouTube)

🎥 **Watch Full ERP System Demo**

> 👉 [Click here to view the live demo on YouTube](https://www.youtube.com/watch?v=YOUR_VIDEO_ID)

> *(Replace `YOUR_VIDEO_ID` with your actual demo video link once uploaded)*

This demo covers:

* Role-based Login
* Center & Class Management
* Child Lifecycle Flow (Review → Active → Archived)
* Waiting List Automation
* Notifications & Reports
* Live Cron Automation Showcase
-->
---
