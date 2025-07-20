Got it! Here’s how to use **ServiceNow REST API Explorer** to **GET incidents** and **CREATE an incident** step-by-step:

---

# Using ServiceNow REST API Explorer for Incident Table

---

## 1. Open REST API Explorer

* Login to your ServiceNow instance.
* In the Application Navigator, type:

  ```
  REST API Explorer
  ```
* Click the **REST API Explorer** module.

---

## 2. Select Table API

* In the REST API Explorer, select:

  * **API**: `Table API`
  * **Table**: `incident`

---

## 3. Get Incidents (GET Request)

* Method: `GET`
* Table Name: `incident`
* Query Parameters (optional):

  * `sysparm_limit=10` (limits results to 10 records)
* Click **Send**

You will get a list of incident records in the response.

---

## 4. Create Incident (POST Request)

* Change Method to: `POST`
* Body (in JSON format), example:

```json
{
  "short_description": "Test incident via REST API Explorer",
  "urgency": "2",
  "impact": "2"
}
```

* Click **Send**

You will get the newly created incident’s details (including `sys_id`, `number`, etc.) in the response.

---

## 5. Authentication

* The REST API Explorer uses your logged-in session, so no extra auth is needed here.
* For external tools like Postman, use Basic Auth with a user who has proper roles (`admin`, `dynatrace_integration`, etc.).

---

## Tips

* You can add filters via query params in GET like:

  ```
  sysparm_query=active=true^priority=1
  ```

* To update an incident, use `PATCH` or `PUT` with the incident `sys_id`.

---

If you want, I can guide you on exporting Postman calls based on REST API Explorer or building automated scripts!
