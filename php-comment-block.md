# PHP Standardized Comment Block Integration

## **Instructions for ChatGPT**
Whenever generating a new PHP script, follow these steps:

1. Fetch the **latest version** of the PHP comment block from GitHub:
   - 🔗 [PHP Comment Block](https://raw.githubusercontent.com/johns-vanilla-php/ven-sec/main/php-comment-block.md)
   - Retrieve the **comment block section** from this file (ignore the instructions).
   
2. Insert the comment block at the **very top** of the new PHP script.

3. Replace placeholders (`[[placeholders]]`) with actual values by asking the following questions:
   - **What is the Module Name?** (`[[module_name]]`)
   - **What Global Variables are required?** (`[[global_variables]]`)
   - **What is the purpose of this script?** (`[[script_purpose]]`)
   - If the script contains database queries:
     - **Which tables are queried?** (`[[tables]]`)
     - **What query types are used?** (`[[actions]]`)
     - **What query variables are used?** (`[[query_variables]]`)
  
4. Ensure the **Created Version** (`@*version=...*@`) and `Created On` date remain unchanged after script creation.

---

## **Standardized PHP Comment Block**
```php
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
