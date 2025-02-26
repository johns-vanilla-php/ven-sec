📌 Master Prompt Template - Ven-Sec Development Framework
🚀 Purpose:

This document serves as the single source of truth for the website development project.
All Individual Prompts must reference this Master Prompt to ensure consistency across all sections.
1. General Website Overview

    Website Name: Ven-Sec
    Primary Purpose: [Briefly describe what the website does]
    Target Audience: [Define user roles & who will use this site]
    Monetization Strategy: [Subscription, one-time payment, free, etc.]
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
🔹 Root Folder: /public_html/

📁 Main Directories:

    assets/ → Stores images, fonts, CSS, and JavaScript.
    functions/ → Stores reusable PHP functions.
    api/ → Stores API endpoints.
    database/ → Stores SQL scripts & backups.
    uploads/ → Stores user-uploaded files.
    sessions/ → Handles user sessions.

📌 Example Folder Hierarchy:

/public_html/
  ├── assets/
  │   ├── css/
  │   ├── js/
  │   ├── images/
  ├── api/
  ├── database/
  ├── functions/
  ├── modules/
  ├── templates/
  ├── uploads/
  ├── index.php

📌 File Naming Conventions:

    CSS: screen_size.css, layout.css, theme.css
    JavaScript: dashboard.js
    PHP: dashboard.php, feature1.php, auth.php

4. Database Schema

📌 All modules must follow the defined database structure.
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

    Save this document in Google Docs, Notion, or a GitHub README.md file.
    When working on new features, copy & paste the Individual Prompt Template into a new chat.
    This ensures that every module stays aligned with the overall project.

Final Steps:

    Copy this formatted content into your Google Doc.
    Save it as your official Master Prompt reference.
    Use it every time you generate an Individual Prompt for new sections of the website.

Now you’re ready to generate structured website components while keeping everything consistent! 🚀 Let me know if you need adjustments.
