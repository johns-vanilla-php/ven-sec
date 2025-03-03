# 📌 Master Prompt Template - Ven-Sec Development Framework

---
## 🚀 Purpose:
This document serves as the **single source of truth** for the website development project.  
All **Individual Prompts** must reference this Master Prompt to ensure **consistency across all sections**.

---

## **1. General Website Overview**
- **Website Name**: Ven-Sec
- **Primary Purpose**:  
  Ven-Sec will allow Security companies to manage their clients, contracts, and employees,  
  allowing real-time management of contracts, clients, worksites, employees, scheduling, and payroll.
- **Target Audience**: Security companies, HR, officers, and clients.
- **Monetization Strategy**: Subscription-based model.
- **Key Technologies**:
  - **Frontend**: HTML5, CSS3, JavaScript (jQuery)
  - **Backend**: PHP8, MySQL, API integrations
  - **Security**: Encryption, Multi-Factor Authentication (MFA)
- **Project Scope**: Version 1 includes core modules for security company management.

---

## **2. Core Features & Modules**
📌 **Each module must align with the Master Prompt and integrate seamlessly into the website.**  

| Feature             | Description                            | User Roles                                                                                                   | Key Integrations              |
| ------------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------ | ----------------------------- |
| User Authentication | Role-based access, session-based login | All Users                                                                                                    | MySQL session storage         |
| Dashboard           | Main UI layout, customizable cards     | **Admin, HR, Clients, Officers, Regional Supervisors, Site Supervisors, Shift Supervisors, Client Liaisons, Company Administrators** | AJAX, CSS themes              |
| QR Code Management  | Generate & scan QR codes               | Admin, Officers, Supervisors                                                                                 | GPS logging, scan logs        |
| Payroll Processing  | Approve timesheets, ADP integration    | HR, Regional Supervisors, Site Supervisors, Shift Supervisors, Officers, Company Administrators             | Payroll logs, ADP API         |
| Contracts & Bidding | Clients post jobs, companies bid       | Clients, Company Administrators                                                                              | Notifications, contract logs  |
| Reports & Logging   | Generate PDF reports, export logs      | Admin, Supervisors, HR                                                                                       | TCPDF, database logs          |
| Notifications       | In-app alerts, email & SMS             | All Users                                                                                                    | Toastr.js, notification table |
| Vehicle Maintenance | Track and manage vehicle maintenance   | Officers, Site Supervisors, HR, Company Administrators                                                       | Maintenance logs, scheduling  |

---

## **3. Folder & File Structure**
💾 **All files must follow this structure to ensure consistency.**  

### 🔹 **Root Folder: `home/x8f5w8389tfu/`**
| **Folder / File**       | **Description** |
|-------------------------|----------------|
| `root/`                | Main project root directory |
| ├── `api/`             | API endpoints for external integrations |
| ├── `backups/`         | Backup storage for database and files |
| ├── `config/`          | Configuration files |
| ├── `cron_jobs/`       | Scheduled task scripts |
| ├── `database_backup/` | Backup of database dumps |
| ├── `engine/`          | Core processing engine |
| │ ├── `db_functions/`  | Database query functions |
| │ ├── `includes/`      | Commonly included PHP files |
| │ ├── `0_start.php`    | Initial boot script |
| │ ├── `1_db_start.php` | Database connection initialization |
| │ ├── `load_engine.php` | Loads core engine components |
| ├── `init/`            | Initialization scripts |
| ├── `logs/`            | System logs |
| │ ├── `http_errors/`   | HTTP error logs |
| │ ├── `code_errors/`   | Application error logs |
| ├── `public_html/`     | Public web directory |
| │ ├── `admin/`         | Admin-specific files |
| │ │ ├── `functions/`   | Admin-related functions |
| │ ├── `assets/`        | Static assets like CSS, JS, fonts, and images |
| │ │ ├── `css/`         | Stylesheets |
| │ │ ├── `fonts/`       | Font files |
| │ │ ├── `images/`      | Image storage |
| │ │ ├── `js/`          | JavaScript files |
| │ ├── `dashboards/`    | User role-based dashboards |
| │ │ ├── `client/`      | Client dashboard |
| │ │ ├── `company/`     | Company dashboard |
| │ │ ├── `hr/`          | HR dashboard |
| │ │ ├── `liaison/`     | Client Liaison dashboard |
| │ │ ├── `officer/`     | Security Officer dashboard |
| │ │ ├── `regional_supervisor/` | Regional Supervisor dashboard |
| │ │ ├── `shift_supervisor/` | Shift Supervisor dashboard |
| │ │ ├── `site_supervisor/` | Site Supervisor dashboard |
| │ ├── `dashboard.php`  | Main dashboard page |
| │ ├── `error.php`      | Error page |
| │ ├── `index.php`      | Website landing page |
| ├── `sessions/`        | Session management files |
| ├── `site_conf/`       | Site configuration settings |
| ├── `storage/`         | Stored files |
| │ ├── `qr_codes/`      | QR code image storage |
| │ ├── `reports/`       | Generated reports |
| │ ├── `uploads/`       | User uploads |
| ├── `vendor/`          | Third-party dependencies (Composer packages) |
| ├── `composer.json`    | Composer dependencies definition file |
| ├── `composer.lock`    | Composer lock file to track exact dependency versions |

---

## **4. Database Schema**
📌 **The full database schema is stored in [`database-schema.sql`](https://github.com/johns-vanilla-php/ven-sec/blob/main/database-schema.sql).**  
For any changes to the database, refer to **the migration system** in `migrations/`.

---

## **5. Role-Based Access Control (RBAC)**
🔑 **Every module must follow role-based permissions.**

| User Role | Access Level                 | Can Edit? | Can View? |
|-----------|------------------------------|-----------|-----------|
| Admin     | Full access                  | ✅        | ✅        |
| HR        | Payroll, employee management | ✅        | ✅        |
| Clients   | Contracts, invoices          | ❌        | ✅        |
| Employees | View pay stubs, schedules    | ❌        | ✅        |

---

## **6. Generating Individual Prompts**
🔹 **How to reference this Master Prompt when creating a new chat:**  
When generating a specific module, use this format:

7. UI/UX Design Standards

🎨 All pages must follow the website’s design system.

    Responsive Design: Mobile-first, scales to all screen sizes.
    ADA Compliance: High-contrast mode, keyboard navigation, larger fonts.
    Customizable Themes: Users can choose from predefined themes.

8. Generating Individual Prompts

🔹 How to reference this Master Prompt when creating a new chat:
When generating a specific module, use this format:

Referencing my **Master Prompt: Website Development Framework for Ven-Sec**, generate a detailed implementation for the [Feature Name] module.

### **Key Details**:
- Ensure it integrates with [Mention Other Modules].
- Role-based access: [Specify roles & permissions].
- Uses database table: [Table name].
- Follows folder structure & UI layout as defined in the Master Prompt.

Example: Individual Prompt for Payroll Module

Referencing my **Master Prompt: Website Development Framework for Ven-Sec**, generate a detailed implementation for the **Payroll Processing Module**.

### Key Details:
- Ensure it integrates with the **Shift Report Module** for worked hours.
- Role-based access: **HR/Admin can edit, Employees can view, Clients have no access**.
- Uses the database table **payroll_records** as defined in the Master Prompt.
- Follows the same **folder structure** and UI layout standards.

How to Use This Document

📌 This Master Prompt should be used as a reference for all generated website sections.

    Save this document in a GitHub README.md file.
    When working on new features, copy & paste the Individual Prompt Template into a new chat.
    This ensures that every module stays aligned with the overall project.

Final Steps:

    Copy this formatted content into your GitHub README.md.
    Save it as your official Master Prompt reference.
    Use it every time you generate an Individual Prompt for new sections of the website.

Now you’re ready to generate structured website components while keeping everything consistent! 🚀 Let me know if you need adjustments.
