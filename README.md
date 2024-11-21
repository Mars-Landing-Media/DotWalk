# DotWalk by [Your Company Name]

DotWalk is a powerful ServiceNow tool designed to extend the capabilities of dot-walking into **GlideAjax scripting**. With DotWalk, developers can dynamically retrieve related fields and records across complex relationships directly from client-side scripts, enabling seamless data access and reducing the need for extensive back-end logic. This tool is perfect for ServiceNow developers looking to streamline data integration and enhance performance.

---

## Features

- **Dynamic Dot-Walking**: Traverse relationships and fetch data using flexible paths (e.g., `manager-department-name`).
- **GlideAjax Integration**: Leverage ServiceNow's GlideAjax for client-server interactions.
- **JSON-Ready Outputs**: Get structured, ready-to-use JSON responses for integration and manipulation.
- **Customizable Queries**: Easily configure tables, queries, and variables for targeted data retrieval.
- **Efficient & Scalable**: Reduce server calls and development complexity, boosting application performance.

---

## Installation

1. Download the `DotWalk` script include and add it to your ServiceNow instance.
2. Create or update your client scripts to use the GlideAjax calls provided in the documentation.
3. Configure your `sysparm_variables` and ensure the naming conventions are followed (see [technical_documentation.txt](technical_documentation.txt)).
4. Test your configurations using the provided test script template ([test_script.txt](test_script.txt)).

---

## Usage

### Example GlideAjax Call

```javascript
// DotWalk Client Script

// 'sysparm_variables' allows dot-walking by using '-'
// Example 1: manager-director-name fetches the manager's director's display name
// Example 2: manager-director returns the manager's director's sysID
// Use 'sysparm_tbl' for the table and 'sysparm_query' for an encoded query, mixing variables if needed.

var dotWalkAjax = new GlideAjax('DotWalk');
dotWalkAjax.addParam('sysparm_name', 'getDotData'); // Updated method name
dotWalkAjax.addParam('sysparm_tbl', 'sys_user'); // Table to query
dotWalkAjax.addParam('sysparm_query', 'employee_number=1234567'); // Encoded query
dotWalkAjax.addParam(
    'sysparm_variables',
    'name,u_vp-email,manager-name,u_director-name,location-country,location-latitude'
); // Variables to fetch with dot-walking
dotWalkAjax.getXML(processResponse); // Callback function

function processResponse(response) {
    var answer = response.responseXML.documentElement.getAttribute('answer');
    answer = JSON.parse(answer); // Parse JSON string to object

    // Set form values using '_' instead of '-' in dot-walked keys
    g_form.setValue('name', answer.name);
    g_form.setValue('email', answer.u_vp_email);
    g_form.setValue('manager', answer.manager_name);
    g_form.setValue('director', answer.u_director_name);
    g_form.setValue(
        'loc',
        'Country: ' + answer.location_country + '    Lat: ' + answer.location_latitude
    );
}
