Absolutely! Here’s a clear, step-by-step guide to **create the `dynatrace_integration` user** in ServiceNow and assign the necessary roles so Dynatrace can connect and work smoothly.

---

# How to Create `dynatrace_integration` User in ServiceNow for Dynatrace Integration

---

## 1. Create the User

1. **Login to your ServiceNow instance** as an admin-capable user.

2. Navigate to:

   ```
   User Administration > Users
   ```

3. Click **New** to create a new user.

4. Fill in the form:

   * **User ID**: `dynatrace_integration`
   * **Name**: `Dynatrace Integration` (or whatever you prefer)
   * **Email**: (optional)
   * **Password**: Set a secure password (you will use this in Dynatrace and Postman)
   * **Active**: Checked (user is active)

5. Save the user.

---

## 2. Assign Required Roles

To enable REST API access and incident creation/updating, assign these roles to the user:

* **`itil`**
  *Allows access to incident and problem management*

* **`web_service_admin`**
  *Allows use of REST APIs and web services*

* **`snc_platform_rest_api_access`**
  *(Important)* Grants permission to use the platform REST API — without this, REST calls will get authentication errors.

---

### How to Assign Roles

1. In the `dynatrace_integration` user record, scroll to the **Roles** related list (at the bottom or side).

2. Click **Edit** or **Edit Roles**.

3. Search and add:

   * `itil`
   * `web_service_admin`
   * `snc_platform_rest_api_access`

4. Save.

---

## 3. Verify User Settings

* Confirm user is **Active**.
* Confirm roles are properly assigned.
* Make sure you know the **username** and **password** for the integration config.

---

## 4. Use This User in Dynatrace Integration Configuration

* When you configure Dynatrace in ServiceNow or via REST API, use:

  * **Username**: `dynatrace_integration`
  * **Password**: The password you set
  * **Instance URL**: Your ServiceNow instance URL (e.g., `https://dev196514.service-now.com`)

---

## Additional Notes

* If you have scoped apps or custom roles, make sure the user has access to the relevant tables (`incident`, `problem`, etc.).
* Always test the REST API connectivity with this user via Postman before connecting Dynatrace.

---

If you want, I can also help you with a Postman test collection or sample scripts for creating incidents using this user.

---

Ready to set it up or want me to walk you through REST API testing with this user?
