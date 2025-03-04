I need to create a new module for the Ven-Sec project. Follow these steps:

1️⃣ **Fetch the latest master prompt** from:
   🔗 [Master Prompt](https://github.com/johns-vanilla-php/ven-sec/blob/main/master-prompt.md)
2️⃣ **Fetch the PHP comment block** from:
   🔗 [PHP Comment Block](https://github.com/johns-vanilla-php/ven-sec/blob/main/php-comment-block.md)
3️⃣ **Fetch the database schema** from:
   🔗 [Database Schema](https://github.com/johns-vanilla-php/ven-sec/blob/main/database-schema.md)

📌 **Module Details**
- **Module Name:** [Enter module name]
- **User Role:** [Enter user role (admin, manager, supervisor, officer, etc.)]
- **Purpose of this module:** [Describe functionality]

📌 **Generate the Following Files (One at a Time)**
1. **Functions** (Standalone scripts stored in `/root/engine/functions/{table_name}/{function-name}.php`)
   - Ask me what functions are needed.
   - Generate each function as a separate PHP script.
   - Ensure **NO include statements** (functions are called by the engine).

2. **Full Page Module** (`/root/public_html/dashboards/{user_role}/modules/{module-name}.php`)
   - This is the main user interface for the module.
   - Uses **AJAX** to interact with functions when needed.
   - Includes **CSS & JavaScript** if applicable.

3. **Dashboard Card** (`/root/public_html/dashboards/{user_role}/cards/{module-name}.php`)
   - This is a small widget shown on the dashboard.
   - Displays **relevant module data** in summary format.

📌 **Each file must:**
- Start with the **standard PHP comment block**.
- Replace **all placeholders (`[[placeholders]]`)** in the comment block.
- Follow **Ven-Sec coding & file structure**.
- Store functions **as separate files** (not combined into one).

📌 **Additional Questions**
- Does this module require database queries? If yes:
  - What **tables** does it interact with?
  - What **query types** (SELECT, INSERT, UPDATE, DELETE)?
  - What **variables** are required in queries?
- Do you need any **API integrations**?

Generate **one file at a time**. Let’s start with the first function file.
