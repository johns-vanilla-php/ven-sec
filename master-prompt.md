📌 Master Prompt Template - Ven-Sec Development Framework
🚀 Purpose:

This document serves as the single source of truth for the website development project.
All Individual Prompts must reference this Master Prompt to ensure consistency across all sections.
1. General Website Overview

    Website Name: Ven-Sec
    Primary Purpose: Ven Sec will allow Security companies to manage their clients, contracts, and employees, allowing real-time management of contracts, clients, worksites, employees, scheduling, and payroll.
    Target Audience: Companies
    Monetization Strategy: Subscription
    Key Technologies:
        Frontend: HTML5, CSS3, JavaScript (jQuery)
        Backend: PHP8, MySQL, API integrations
        Security: Encryption, Multi-Factor Authentication (MFA)
    Project Scope: [Define what will be built in version 1]

2. Core Features & Modules

📌 Each module must align with the Master Prompt and integrate seamlessly into the website.
Feature	Description	User Roles	Key Integrations
User Authentication	Role-based access, session-based login	All Users	MySQL session storage
Dashboard	Main UI layout, customizable cards	Admin, HR, Clients	AJAX, CSS themes
QR Code Management	Generate & scan QR codes	Admin, Officers	GPS logging, scan logs
Payroll Processing	Approve timesheets, ADP integration	HR, Employees	Payroll logs, ADP API
Contracts & Bidding	Clients post jobs, companies bid	Clients, Companies	Notifications, contract logs
Reports & Logging	Generate PDF reports, export logs	Admin, Supervisors	TCPDF, database logs
Notifications	In-app alerts, email & SMS	All Users	Toastr.js, notification table
3. Folder & File Structure

💾 All files must follow this structure to ensure consistency.
🔹 Root Folder: /home/$gd_user/

📁 Main Directories:

    assets/ → Stores images, fonts, CSS, and JavaScript.
    functions/ → Stores reusable PHP functions.
    api/ → Stores API endpoints.
    database/ → Stores SQL scripts & backups.
    uploads/ → Stores user-uploaded files.
    sessions/ → Handles user sessions.

📌 Example Folder Hierarchy:

/home/$gd_user/

  ├── api/
  ├── backups/
  ├── config/
  ├── cron_jobs/
  ├── database/
  ├── engine/
  │   │   ├── functions/
  ├── init/
  ├── logs/
  ├── private/
  ├── public_html/
  │   ├── admin/
  │   │   ├── functions/
  │   ├── assets/
  │   │   ├── css/
  │   │   ├── fonts/
  │   │   ├── images/
  │   │   │   ├── qr_codes/
  │   │   ├── js/
  │   ├── dashboards/
  │   │   │   ├── client/
  │   │   │   ├── company/
  │   │   │   ├── hr/
  │   │   │   ├── liaison/
  │   │   │   ├── manager/
  │   │   │   ├── officer/
  │   │   │   ├── regional_supervisor/
  │   │   │   ├── site_supervisor/
  │   │   │   ├── shift_supervisor/
  │   │   │   ├── vs_admin/
  │   ├── .htaccess
  │   ├── conf_init.php
  │   ├── assets/
  │   ├── assets/
  │   ├── assets/
  │   ├── modules/
  │   ├── templates/
  │   ├── index.php
  │   ├── login.php
  │   ├── logout.php
  │   ├── timeout.php
  │   ├── session_auth.php
  │   ├── error.php
  ├── reports/
  ├── site_conf/
  ├── storage/
  ├── vendor/
  ├── index.php

📌 File Naming Conventions:

    CSS: screen_size.css, layout.css, theme.css
    JavaScript: dashboard.js
    PHP: dashboard.php, feature1.php, auth.php

4. Database Schema

📌 All modules must follow the defined database structure.  Do not add constraints or foreign keys.
Table Name	Purpose	Key Fields
users	Stores user info	id, name, email, role, password_hash
qr_scans	Logs QR code scans	id, user_id, qr_code_id, scan_time, location
payroll_records	Stores payroll data	id, employee_id, hours_worked, pay_period, status
contracts	Manages contract bidding	id, client_id, company_id, budget, status
5. Role-Based Access Control (RBAC)

🔑 Every module must follow role-based permissions.
User Role	Access Level	Can Edit?	Can View?
Admin	Full access	✅	✅
HR	Payroll, employee management	✅	✅
Clients	Contracts, invoices	❌	✅
Employees	View pay stubs, schedules	❌	✅
6. Integration & Communication Between Sections

🛠 Every module must integrate with the website’s core systems.

    AJAX-based dynamic content loading
    Standardized UI components
    Consistent database relationships
    Cross-module interaction (e.g., Payroll pulls from Shift Reports)

7. UI/UX Design Standards

🎨 All pages must follow the website’s design system.

    Responsive Design: Mobile-first, scales to all screen sizes.
    ADA Compliance: High-contrast mode, keyboard navigation, larger fonts.
    Customizable Themes: Users can choose from predefined themes.

8. PHP Script Comment Block Standard

Whenever generating a new PHP script, include the following standard comment block at the top of the file.

    Manually Updated Fields:
        Created Version (@*version=...*@) remains manually updated.
        Created On (@*created=...*@) stays the same after creation.
    Automatically Updated Fields:
        Last Modified updates whenever the script is edited.
        Module Version (@*moduleversion=...*@) follows semantic versioning:
            Major updates (new features, structure changes) → X.0.0 → X.1.0
            Minor updates (bug fixes, small enhancements) → X.X.0 → X.X.1
        @*changelog=...*@ logs only the latest update.
        Full changelog history is maintained in descending order.

PHP Comment Block Template:

<?php
/*//////////////////////////////////////////////////////////////////////////////////////////////////

Authors:          John Stuttler
Created On:       [[created_date]]
Created Version:  1.0.0  <!-- This remains manually updated -->
Last Modified:    [[modified_date]]
Module:           [[module_name]]
Module Version:   [[module_version]]

Changelog:
[[changelog_history]]

*///////////////////////////////////////////////////////////////////////////////////////////////////
/**
 @*needs_reviewed=yes*@
 @*authors=John Stuttler*@
 @*created=[[created_date]]*@
 @*modified=[[modified_date]]*@
 @*version=1.0.0*@  <!-- This remains manually updated -->
 @*module=[[module_name]]*@
 @*moduleversion=[[module_version]]*@
 @*changelog=[[latest_changelog_entry]]*@
 @*needs_reviewed=yes*@
 @*databases=vs_db*@
 @*tables=[[tables]]*@
 @*actions=[[actions]]*@
 @*variables=[[query_variables]]*@
 @*globals=[[global_variables]]*@
 @*see_also=*@
<div>
  <table class="commentTable">
    <tr>
      <td class="commentCell">
        <p>
          [[script_purpose]]
        </p>
      </td>
    </tr>
  </table>
</div>
 */

3. Questions to Ask Before Generating a PHP Script

Before generating a PHP script, always ask the following questions:

    What is the Module Name? (Replaces [[module_name]])
    What Global Variables are required? (Replaces [[global_variables]])
    What is the purpose of this script? (Replaces [[script_purpose]])
    Does this script contain database queries?
        If yes, determine:
            Which tables are queried? (Replaces [[tables]])
            What query types are used? (Insert, Update, Select, etc.) (Replaces [[actions]])
            What query variables are used? (Replaces [[query_variables]])
                Example:

string $is_active (Optional - 'yes' or 'no')
,float $price_each (Default - 0.00)
,int $quantity (Default - 0)
