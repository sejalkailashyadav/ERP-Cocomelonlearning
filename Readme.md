# Childcare Management ERP

### Overview

This **ERP (Enterprise Resource Planning)** system is designed for managing multiple centers efficiently.
It provides **role-based access** for Super Admins, Admins, and Managers to handle centers, employees, children, classes, fees, reports, and documentation—all from centralized dashboard.

---

## Roles & Permissions

### 1. Super Admin

* Has **full control** over the entire ERP system.
* Can **create, edit, and delete** Admins and Managers.
* Manages all centers, employees, classes, and reports.
* Access to **Role & Permission CRUD** for assigning roles (Admin/Manager) to specific centers.

#### Role & Permission Module

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

* Create Role
* Edit Role
* Delete Role
* View Assigned Permissions

---

## Core Modules

### 1. Dashboard Management

#### Super Admin Dashboard

* Displays overall summary across all centers.
* KPIs: Total Centers, Active Children, Total Fees Collected, Active Managers, Pending Approvals.

#### Admin Dashboard

* Displays stats of their **specific center**.
* KPIs: Classes, Active Students, Fees Summary, Monthly Reports.

#### Manager Dashboard

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

* Create
* Edit
* Delete
* View Details

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

* Create
* Edit
* Delete

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

* Add Fee Plan
* Edit Fee Plan
* Delete Fee Plan

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

### Status Logic

| Status        | Description                             | Updated By      | Effect on Reports                |
| ------------- | --------------------------------------- | --------------- | -------------------------------- |
| **Review**    | Default when created, pending approval. | System          | Not shown in reports.            |
| **Active**    | Approved and currently enrolled.        | Admin / Manager | Included in monthly reports.     |
| **Withdrawn** | Child leaves the center.                | Admin / Manager | Included for current month only. |
| **Archived**  | Automatically archived post month-end.  | System          | Excluded from active reports.    |

#### Operations:

* Create Child
* Edit Child
* Delete Child
* Approve Child
* Auto Archive (via cron)

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

* Add Employee
* Edit Employee
* Delete
* Auto Import from WordPress Form (via Cron Job)

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

### 10. Wage Report

#### Purpose:

Generate center-wise and month-wise wage reports for employees.

#### Fields Shown in Wage Report:

* Employee Name
* Status
* Designation
* ECE License (No · Exp Date)
* Type
* Hours
* Salary
* Wage
* Total
* Notes

#### Features:

* When user selects a **Center** and **Month**, show employee data:
  * Employee Name
  * Status
  * Designation
  * ECE License (No · Exp Date)
  * Type
  * Hours
  * Salary
  * Wage
  * Total
  * Notes
* While creating wage report, user can **rename report** similar to Fees Report.
* Editable fields:
  * Designation
  * Type
  * Select Centers
  * Hours
  * Salary
  * Wage *(default $6)*
  * Notes
* During report creation, employees can be excluded using checkbox selection.
* Admin can add/edit static new employee entries in the list, and those entries apply only to that particular report.
* Clicking on employee name redirects to employee respective edit page.
* **Qualify for Wage Enhancement**
  * If selected as **Yes**, then show in report.
  * Otherwise, do not generate that employee in that report itself.

#### Wage Listing Page:

Fields include:

* Employee Name
* Status
* Designation
* ECE License (No · Exp Date)
* Certification Validation
* Contract Document Type
* Hours
* Salary
* Wage
* Total
* Notes

---

### 11. Timeline & Task Management

#### Features:

* The Super Admin can create and assign tasks to any system user.
* Each task includes:
  * Date
  * Time
  * Assigned User
  * Last Updated Time
* Users can mark tasks as:
  * Completed
  * Pending
* Users can also add notes.
* The Super Admin can add, edit, and delete tasks.
* Managers can view tasks and update their status:
  * Overdue
  * Pending
  * Completed
* Recurring tasks are automatically generated based on a schedule.
* All tasks are displayed in a **timeline view**, and notes are shown in a **thread view**.
* The admin receives an **email notification one day before a task’s due date**.
* Manager-wise email addresses can be added to specify managers for a particular center.

---

### 12. Notification Section

#### Child Notifications

Approval workflow includes:

* Create child request
* Approval
* Approve request
* Withdrawal
* Withdrawal request
* Delete
* Delete request

---

### 13. Summary of Employee Master

#### Sent on the 1st of Every Month

1. New Joiners (Last Month)
2. Completed Probation (Last Month)
3. Last Day of Working (Last Month)
4. Expired Documents (Last Month)
5. Upcoming Completed Probation (Upcoming Month)
6. Upcoming Expiring Documents (Upcoming Month)

---

### 14. Dashboard Enhancements

#### Employee and Document Summary Widgets

* New Joiners (Monthly Summary)
* Completed Probation (Monthly Summary)
* Last Day of Working
* Expired Documents
* Upcoming Completed Probation
* Upcoming Expiring Documents
* Displayed on the dashboard along with quick links

---

## Manager Role (Limited Access)

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

## Automation (Cron Jobs)

1. **Withdrawn → Archived Conversion**

   * Runs monthly at end of the month.
   * Moves all *Withdrawn* children to *Archive* state.

2. **Website → ERP Data Sync**

   * Fetches new employee/child form data from WordPress and inserts into ERP Waiting List.

3. **Recurring Task Auto Generation**

   * Automatically creates recurring tasks based on configured schedules.

4. **Task Due Reminder Notification**

   * Sends email reminder to admin one day before a task due date.

5. **Monthly Employee Master Summary**

   * Sent on the 1st of every month with employee movement and document status summary.

---

## Summary Table

| Module                   | CRUD | Role Access                 | Automation                 |
| ------------------------ | ---- | --------------------------- | -------------------------- |
| Dashboard                | View | All                         | —                          |
| Center Management        | Yes  | Super Admin, Admin          | —                          |
| Class Management         | Yes  | Super Admin, Admin, Manager | —                          |
| Fees Master              | Yes  | Super Admin, Admin          | —                          |
| Child Master             | Yes  | Super Admin, Admin, Manager | Withdrawn→Archive          |
| Report Master            | Yes  | Super Admin, Admin, Manager | —                          |
| Wage Report              | Yes  | Super Admin, Admin          | —                          |
| Employee Master          | Yes  | Super Admin                 | Monthly Summary            |
| Employee Waiting List    | Yes  | Super Admin, Admin          | Website→ERP Sync           |
| Waiting List             | Yes  | Super Admin, Admin          | Website→ERP Sync           |
| Timeline & Task Mgmt     | Yes  | Super Admin, Manager        | Recurring Tasks, Reminders |
| Notification Section     | View | Super Admin, Admin, Manager | Approval Flow Alerts       |
| Role & Permission        | Yes  | Super Admin                 | —                          |

---

## Tech Stack (for context)

* **Tech Stack:** Node.js, Express, React
* **Database:** MySQL
* **Hosting::** Hostinger Shared Hosting (Canada Server, .ca domain)
* **Storage::** Local (Hostinger Server)
* **Automation:** Cron Jobs (monthly, hourly)

---
## Demo Access

This ERP system is a private internal platform used for managing centers, employees, and operations.

 Demo access is restricted due to data privacy and security reasons.

For demonstration or walkthrough, please contact the administrator. sejalyadav122@gmail.com
