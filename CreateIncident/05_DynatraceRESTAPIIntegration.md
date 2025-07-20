Thanks for pointing that out — you're likely seeing Dynatrace's **newer simplified webhook UI**, which doesn't show method/body/headers immediately unless you enable **Advanced Settings**.

Let me walk you through exactly what to do so you can configure the **ServiceNow integration** properly.

---

# ✅ Dynatrace Webhook Integration for ServiceNow – Full Setup (Advanced Mode)

---

## 🔹 Step-by-Step in Dynatrace UI

### 1. Go to Dynatrace:

```
Settings → Integration → Problem notifications
```

### 2. Click: **“Add notification” → “Custom integration”**

---

## 🔹 Basic Information

* **Name**: `ServiceNow Incident Integration`

---

## 🔹 Enable Advanced Settings

> 🔧 Click **“Advanced setup”** or **toggle the "Use custom payload" / "Advanced options"** switch — this reveals the HTTP method, headers, and body fields.

If you don’t see these:

* You’re on a limited UI — check if you can switch to **custom integration** or click **“Switch to advanced”**

---

## 🔹 Fill in These Fields:

### ✅ **Webhook URL**:

```
https://<your-instance>.service-now.com/api/now/table/incident
```

Example:

```
https://dev196514.service-now.com/api/now/table/incident
```

---

### ✅ **HTTP Method**:

```
POST
```

---

### ✅ **HTTP Headers**:

Add two headers:

| Key          | Value            |
| ------------ | ---------------- |
| Content-Type | application/json |
| Accept       | application/json |

---

### ✅ **Authentication**:

* Use **Basic Auth**
* Username: `dynatrace_integration`
* Password: the one you set in ServiceNow

---

### ✅ **Custom Request Body** (JSON)

Example dynamic payload with Dynatrace tokens:

```json
{
  "short_description": "Dynatrace Problem: {ProblemTitle}",
  "description": "Problem detected in Dynatrace.\n\nSeverity: {ProblemSeverity}\nImpact: {Impact}\nAffected entities: {ImpactedEntities}\n\nDetails: {ProblemDetails}\nLink: {ProblemURL}",
  "urgency": "2",
  "impact": "2",
  "category": "inquiry"
}
```

> 📝 Customize the fields based on your ServiceNow schema and desired behavior.

---

### ✅ Test It!

1. Scroll down and click **“Send test notification”**

2. Check the response:

   * HTTP **201 Created** = ✅ success
   * HTTP **401/403** = auth issue
   * HTTP **400** = bad body/payload

3. Open ServiceNow and verify a new incident was created.

---

## ✅ Optional: Troubleshooting Checklist

| Issue               | Fix                                       |
| ------------------- | ----------------------------------------- |
| 401 Unauthorized    | Check username/password and roles in SNOW |
| 400 Bad Request     | Confirm JSON payload format is valid      |
| No incident created | Use Postman to debug first                |

---

