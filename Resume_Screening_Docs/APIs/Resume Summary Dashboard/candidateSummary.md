# 1. Filter Candidates by AI Score (GET)

Retrieves candidates whose AI score is greater than or equal to the specified minimum score. The maximum score is always **100%**.

* **Endpoint:** `/candidates/filter/score`
* **Method:** `GET`
* **Auth Required:** Yes (JWT)

#### **Query Parameters**

| Field | Type | Description |
| :--- | :--- | :--- |
| `score` | `number` | **(Required)** Minimum AI score percentage. Allowed values: `0` to `100`. Candidates with an AI score between the specified value and **100** are returned. |

#### **Request Headers**

| Field | Value | Description |
| :--- | :--- | :--- |
| `Authorization` | `Bearer <JWT_TOKEN>` | The token received from the Login API. |

#### **Example Request URL**

Returns all candidates with an AI score of **70% or higher**.

```http
GET /candidates/filter/score?score=70
```

---

#### **Response**

**Success (200 OK):**

```json
{
  "status": "success",
  "message": "Candidates retrieved successfully.",
  "filter": {
    "score": 70
  },
  "data": [
    {
      "candidateId": "cand_001",
      "candidateName": "John Doe",
      "aiScore": 92
    },
    {
      "candidateId": "cand_002",
      "candidateName": "Jane Smith",
      "aiScore": 85
    },
    {
      "candidateId": "cand_008",
      "candidateName": "Michael Brown",
      "aiScore": 74
    }
  ]
}
```

---

**Error (400 Bad Request):**

Occurs if the score is outside the allowed range.

```json
{
  "status": "error",
  "message": "Invalid score. Score must be between 0 and 100."
}
```

---

**Error (401 Unauthorized):**

Occurs if the token is missing, expired, or invalid.

```json
{
  "status": "error",
  "message": "Unauthorized access. Please login again."
}
```

---

**Error (404 Not Found):**

Occurs if no candidates have an AI score greater than or equal to the specified score.

```json
{
  "status": "error",
  "message": "No candidates found matching the specified AI score."
}
```

# 2. Filter Candidates by Status (GET)

Retrieves candidates based on their current recruitment status.

* **Endpoint:** `/candidates/filter/status`
* **Method:** `GET`
* **Auth Required:** Yes (JWT)

#### **Query Parameters**

| Field | Type | Description |
| :--- | :--- | :--- |
| `status` | `string` | **(Required)** Allowed values: `holds`, `selected`, `rejected`. |

#### **Request Headers**

| Field | Value | Description |
| :--- | :--- | :--- |
| `Authorization` | `Bearer <JWT_TOKEN>` | The token received from the Login API. |

#### **Example Request URL**

```http
GET /candidates/filter/status?status=selected
```

#### **Response (200 OK)**

```json
{
  "status": "success",
  "message": "Candidates retrieved successfully.",
  "data": [
    .
    .
    .
    .
  ]
}
```

---

# 3. Filter Candidates by Experience Level (GET)

Retrieves candidates based on their experience level.

* **Endpoint:** `/candidates/filter/experience`
* **Method:** `GET`
* **Auth Required:** Yes (JWT)

#### **Query Parameters**

| Field | Type | Description |
| :--- | :--- | :--- |
| `experienceLevel` | `string` | **(Required)** Allowed values: `fresher`, `1-3`, `3-5`, `5+`. |

#### **Request Headers**

| Field | Value | Description |
| :--- | :--- | :--- |
| `Authorization` | `Bearer <JWT_TOKEN>` | The token received from the Login API. |

#### **Example Request URL**

```http
GET /candidates/filter/experience?experienceLevel=3-5
```

#### **Response (200 OK)**

```json
{
  "status": "success",
  "message": "Candidates retrieved successfully.",
  "data": [
    {
      "candidateId": "cand_002",
      "candidateName": "Jane Smith",
      "experience": "4 Years",
      "experienceLevel": "3-5"
    },
    {
      "candidateId": "cand_010",
      "candidateName": "Michael Lee",
      "experience": "5 Years",
      "experienceLevel": "3-5"
    }
  ]
}
```

---
# 5. Save Candidate Note (POST)

Saves a note for a specific candidate.

* **Endpoint:** `/candidates/{candidateId}/notes`
* **Method:** `POST`
* **Auth Required:** Yes (JWT)

#### **Path Parameters**

| Field | Type | Description |
| :--- | :--- | :--- |
| `candidateId` | `string` | **(Required)** Unique ID of the candidate. |

#### **Request Headers**

| Field | Value | Description |
| :--- | :--- | :--- |
| `Authorization` | `Bearer <JWT_TOKEN>` | The token received from the Login API. |
| `Content-Type` | `application/json` | Indicates that the request body contains JSON data. |

#### **Request Body**

| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `note` | `string` | Yes | The complete note entered by the user. Supports long-form text and multiple paragraphs. |

#### **Example Request**

```http
POST /candidates/cand_001/notes
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

```json
{
  "note": ".............................."
}
```

---

#### **Response**

**Success (201 Created):**

```json
{
  "status": "success",
  "message": "Candidate note saved successfully.",
  "data": {
    "noteId": "note_001",
    "candidateId": "cand_001",
    "createdAt": "2026-08-03T10:15:30Z"
  }
}
```

---

**Error (400 Bad Request):**

Occurs if the request body is missing the `note` field or it is empty.

```json
{
  "status": "error",
  "message": "The note field is required."
}
```

---

**Error (401 Unauthorized):**

Occurs if the JWT token is missing, expired, or invalid.

```json
{
  "status": "error",
  "message": "Unauthorized access. Please login again."
}
```

---

**Error (404 Not Found):**

Occurs if the specified candidate does not exist.

```json
{
  "status": "error",
  "message": "Candidate not found."
}
```

## 6. Update Candidate Status (PATCH)

Updates the status of a specific candidate. When a user clicks the **Select**, **Reject**, or **Hold** button, the candidate's status is updated in the database.

* **Endpoint:** `/candidates/{candidateId}/status?"`
* **Method:** `PATCH`
* **Auth Required:** Yes (JWT)

#### **Path Parameters**

| Field | Type | Description |
| :--- | :--- | :--- |
| `candidateId` | `string` | **(Required)** Unique ID of the candidate. |

#### **Request Headers**

| Field | Value | Description |
| :--- | :--- | :--- |
| `Authorization` | `Bearer <JWT_TOKEN>` | The token received from the Login API. |
| `Content-Type` | `application/json` | Indicates that the request body contains JSON data. |

#### **Request Body**

| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `status` | `string` | Yes | New candidate status. Allowed values: `selected`, `rejected`, `holds`. |

#### **Example Request**

```http
PATCH /candidates/cand_001/status
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

```json
{
  "status": "selected"
}
```

---

#### **Response**

**Success (200 OK):**

```json
{
  "status": "success",
  "message": "Candidate status updated successfully.",
  "data": {
    "candidateId": "cand_001",
    "candidateStatus": "selected",
    "updatedAt": "2026-08-03T11:30:15Z"
  }
}
```

---

**Error (400 Bad Request):**

```json
{
  "status": "error",
  "message": "Invalid status. Allowed values are: selected, rejected, holds."
}
```

---

**Error (401 Unauthorized):**

```json
{
  "status": "error",
  "message": "Unauthorized access. Please login again."
}
```

---

**Error (404 Not Found):**

```json
{
  "status": "error",
  "message": "Candidate not found."
}
```

---


# 7. Save Candidate CTC (POST)

Saves the  CTC for a specific candidate. 

* **Endpoint:** `/candidates/{candidateId}/ctc`
* **Method:** `POST`
* **Auth Required:** Yes (JWT)

#### **Path Parameters**

| Field | Type | Description |
| :--- | :--- | :--- |
| `candidateId` | `string` | **(Required)** Unique ID of the candidate. |

#### **Request Headers**

| Field | Value | Description |
| :--- | :--- | :--- |
| `Authorization` | `Bearer <JWT_TOKEN>` | The token received from the Login API. |
| `Content-Type` | `application/json` | Indicates that the request body contains JSON data. |

#### **Request Body**

| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `ctc` | `number` | Yes | Candidate's offered CTC (Annual Cost to Company). |

#### **Example Request**

```http
POST /candidates/cand_001/ctc
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

```json
{
  "ctc": 1200000
}
```

---

#### **Response**

**Success (201 Created):**

```json
{
  "status": "success",
  "message": "Candidate CTC saved successfully.",
  "data": {
    "candidateId": "cand_001",
    "ctc": 1200000,
    "currency": "INR",
    "savedAt": "2026-08-03T11:45:20Z"
  }
}
```

---

**Error (400 Bad Request):**

```json
{
  "status": "error",
  "message": "Invalid CTC value. CTC must be greater than zero."
}
```

---

**Error (401 Unauthorized):**

```json
{
  "status": "error",
  "message": "Unauthorized access. Please login again."
}
```

---

**Error (404 Not Found):**

```json
{
  "status": "error",
  "message": "Candidate not found."
}
```