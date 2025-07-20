Great question! To **automatically close an incident in ServiceNow** when a **Dynatrace problem is resolved**, you'll need to implement a **two-way integration**. Dynatrace can send a second webhook notification on **problem resolution**, and you use that to update the incident in ServiceNow (e.g., mark it as resolved/closed).

---

# ✅ Step-by-Step: Close ServiceNow Incident from Dynatrace

---

## 🧩 Goal

* When Dynatrace **detects a problem** → Create a new incident in SNOW (✅ you already did this)
* When Dynatrace **resolves the problem** → Send another webhook to **update** the existing SNOW incident and mark it as **Resolved/Closed**

---

## 🔁 High-Level Workflow

1. **Dynatrace Problem OPEN** → `POST` → Create incident (✅ done)
2. **Dynatrace Problem RESOLVED** → `PATCH` or `PUT` → Update incident (set `state = 6` or `7`)

---

## 🔧 Step-by-Step in Dynatrace (Two Webhooks)

---

### 🔹 1. Create a Second Webhook Notification

In Dynatrace:

* Go to:

  ```
  Settings → Integration → Problem Notifications
  ```

* Click **Add notification** → **Custom integration**

* Name it:

  ```
  ServiceNow Close Incident
  ```

* Enable **Advanced Settings / Custom Payload**

---

### 🔹 2. Configure Request Details

#### ✅ Webhook URL

```http
https://<your-instance>.service-now.com/api/now/table/incident?sysparm_query=short_descriptionLIKE{ProblemTitle}
```

> This finds the incident created earlier using the problem title.

---

#### ✅ HTTP Method

```
PATCH
```

> `PATCH` is used to update specific fields of an existing record.

---

#### ✅ HTTP Headers

| Header       | Value            |
| ------------ | ---------------- |
| Content-Type | application/json |
| Accept       | application/json |

---

#### ✅ Authentication

* **Basic Auth**
* Username: `dynatrace_integration`
* Password: your SNOW password

---

#### ✅ Request Body (JSON)

```json
{
  "state": "7",
  "close_code": "Closed/Resolved by Caller",
  "close_notes": "Closed automatically by Dynatrace on problem resolution"
}
```

* `state = 6` = Resolved
* `state = 7` = Closed
  *(choose based on your SNOW workflow)*

---

### 🔹 3. Trigger: Problem Closed

Scroll down and set **trigger** to:

```
Send only when problem is resolved
```

Then click **Send test notification** (you can simulate this by closing a test problem).

---

## 🧪 Alternative (More Reliable): Use sys\_id from Initial POST

If your initial POST (create incident) stores the `sys_id` in Dynatrace **custom tags** or logs it somewhere, you can:

* Use that exact `sys_id` in your second webhook:

```http
PATCH https://<instance>.service-now.com/api/now/table/incident/<sys_id>
```

This is **more reliable** than matching by `short_description`.

---

## ✅ Result

When a Dynatrace problem is resolved:

* Dynatrace sends a `PATCH` request to ServiceNow
* The matching incident is **closed or resolved**
* Done! 🎉

---

## ⚠️ Important Notes

* Use **`sysparm_query`** carefully — avoid closing unrelated incidents.
* You can add custom logic or tags in Dynatrace to store mapping between problem and incident.

---

## 🔧 Want Help With:

* A two-integration sample (open + close)?
* A webhook test collection in Postman?
* Storing `sys_id` during creation for later closing?

Let me know and I’ll build it for you.
