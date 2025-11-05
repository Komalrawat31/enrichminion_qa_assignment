# 🧪 Postman API Automation Test Scripts – EnrichMinion

This document includes automation test scripts covering:

- Authentication & Token Validation  
- Status Codes (200, 400, 401, 500)  
- Schema Validation  
- Security Checks  
- Error Message Consistency  

---

## **1️⃣ Signup – Valid User**
**Endpoint:** `POST https://xcyaxtcjarezzxcxklnu.supabase.co/auth/v1/signup`

**Request Body:**
```json
{
  "email": "testuser@example.com",
  "password": "Test@12345"
}
```

**Tests:**
```js
pm.test("Status code is 200", () => {
  pm.response.to.have.status(200);
});

pm.test("Response contains user object", () => {
  const res = pm.response.json();
  pm.expect(res).to.have.property("user");
  pm.expect(res.user).to.have.property("id");
});
```

---

## **2️⃣ Login – Valid Credentials**
**Endpoint:** `POST https://xcyaxtcjarezzxcxklnu.supabase.co/auth/v1/token?grant_type=password`

**Request Body:**
```json
{
  "email": "testuser@example.com",
  "password": "Test@12345"
}
```

**Tests:**
```js
pm.test("Status code is 200", () => {
  pm.response.to.have.status(200);
});

pm.test("Access token generated", () => {
  const res = pm.response.json();
  pm.expect(res).to.have.property("access_token");
  pm.environment.set("token", res.access_token);
});

pm.test("Schema validation for login", () => {
  pm.response.to.have.jsonSchema({
    type: "object",
    required: ["access_token", "token_type", "expires_in"],
    properties: {
      access_token: { type: "string" },
      token_type: { type: "string" },
      expires_in: { type: "number" }
    }
  });
});
```

---

## **3️⃣ Login – Invalid Password**
**Endpoint:** same as above  
**Request Body:**
```json
{
  "email": "testuser@example.com",
  "password": "WrongPassword"
}
```

**Tests:**
```js
pm.test("Status code 400 for invalid credentials", () => {
  pm.response.to.have.status(400);
});

pm.test("Error message consistency", () => {
  const res = pm.response.json();
  pm.expect(res.code).to.eql("invalid_credentials");
  pm.expect(res.message).to.eql("Invalid login credentials");
});
```

---

## **4️⃣ Google Signup (OAuth)**
**Endpoint:**  
`GET https://xcyaxtcjarezzxcxklnu.supabase.co/auth/v1/authorize?provider=google&redirect_to=https://enrichminion.vercel.app/enrichment/phone-finder`

**Tests:**
```js
pm.test("Status code 400 - Unsupported Provider", () => {
  pm.response.to.have.status(400);
});

pm.test("Error message mentions provider issue", () => {
  const res = pm.response.json();
  pm.expect(res.msg).to.include("Unsupported provider");
});
```

---

## **5️⃣ Enrichment API**
**Endpoint:** `POST https://dev-api.enrichminion.com/api/enrichment`

**Headers:**  
`Authorization: Bearer {{token}}`  
`Content-Type: application/json`

**Request Body:**
```json
{
  "file_name": "Test_File.csv"
}
```

**Tests:**
```js
pm.test("Status code 201 Created", () => {
  pm.response.to.have.status(201);
});

pm.test("Schema validation for enrichment", () => {
  pm.response.to.have.jsonSchema({
    type: "object",
    required: ["id", "status", "data"],
    properties: {
      id: { type: "string" },
      status: { type: "string" },
      data: { type: "object" }
    }
  });
});
```

---

## **6️⃣ Verification API**
**Endpoint:** `POST https://dev-api.enrichminion.com/api/verification/execute/file`

**Headers:**  
`Authorization: Bearer {{token}}`  
`Content-Type: application/json`

**Request Body:**
```json
{
  "file_name": "Verification_Test.csv"
}
```

**Tests:**
```js
pm.test("Status code 201 Created", () => {
  pm.response.to.have.status(201);
});

pm.test("Response schema validation", () => {
  pm.response.to.have.jsonSchema({
    type: "object",
    required: ["verification_id", "status"],
    properties: {
      verification_id: { type: "string" },
      status: { type: "string" }
    }
  });
});
```

---

## **7️⃣ Unauthorized Enrichment Access**
**Endpoint:** `POST https://dev-api.enrichminion.com/api/enrichment`  
(without `Authorization` header)

**Tests:**
```js
pm.test("Status code 401 Unauthorized", () => {
  pm.response.to.have.status(401);
});

pm.test("Error message is clear", () => {
  const res = pm.response.json();
  pm.expect(res.message).to.match(/token missing|invalid/i);
});
```

---

## **8️⃣ Security Header Validation**
**Tests:**
```js
pm.test("Response contains security headers", () => {
  pm.expect(pm.response.headers.has("access-control-allow-origin")).to.be.true;
  pm.expect(pm.response.headers.get("content-security-policy")).to.not.be.undefined;
});
```

---

## **9️⃣ Response Time Validation**
**Tests:**
```js
pm.test("Response time < 1000ms", () => {
  pm.expect(pm.response.responseTime).to.be.below(1000);
});
```

---

## **🔟 Schema & Error Consistency for Missing Params**
**Tests:**
```js
pm.test("Returns 400 for missing required fields", () => {
  pm.response.to.have.status(400);
});

pm.test("Error message mentions missing fields", () => {
  const res = pm.response.json();
  pm.expect(res.message).to.match(/missing|required/i);
});
```

---

## ✅ How to Use in Postman
1. Open your Postman collection.  
2. Add each request (Signup, Login, Enrichment, etc.).  
3. Copy and paste the corresponding script under the **Tests** tab.  
4. Run via **Collection Runner** or integrate in CI/CD pipeline.
