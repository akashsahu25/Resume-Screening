## 1. Get Dashboard (GET)

Retrieves the authenticated user's dashboard statistics.

Endpoint: /auth/dashboard

Method: GET

Auth Required: Yes (JWT)

Request Headers

| Field         | Type   | Required | Description                                      |
| :------------ | :----- | :------- | :----------------------------------------------- |
| Authorization | String | Yes      |<your_jwt_token>` |


### Response

**Success (200 OK):**

```json
{
  "status": "success",
  "message": "Dashboard data retrieved successfully",
  "data": {
    "user": {
      "userId": "usr_987654321",
      "fullName": "Jane Doe",
      "email": "jane.doe@example.com"
    }
  }
}
```

**Error (401 Unauthorized):**  
*Occurs when the token is missing or incorrectly formatted.*

```json
{
  "status": "error",
  "message": "Unauthorized access. Please provide a valid token."
}
```

**Error (403 Forbidden):**  
*Occurs when the token is expired or invalid.*

```json
{
  "status": "error",
  "message": "Token has expired. Please login again."
}
```

---
## 2. List Posted Jobs (GET)
Retrieves a list of available jobs. 

*   **Endpoint:** `/Jobs`
*   **Method:** `GET`
*   **Auth Required:** Yes (JWT)

#### Request Headers
| Field | Value | Description |
| :--- | :--- | :--- |
| **Authorization** | `JWT` | The token received from the Login API. |
||


#### Response
**Success (200 OK):**
```json
{
  "status": "success",
  "message": "Jobs retrieved successfully",
  "data": [
    {
      "jobId": "job_001",
      "title": "Software Engineer",
      "Must have skill": "Java, springBoot",
      "applicants": "214",
      "shortlisted": "24",
      "postedDate": "2023-10-25"
    },
    {
      "jobId": "job_002",
      "title": "Product Designer",
      "Must have skill": "Java, springBoot",
      "applicants": "214",
      "shortlisted": "24",
      "postedDate": "2023-10-24"
    }
    .
    .
    .
    .
    .
    .
    .
    .
    .
    .
  ]
}
```

**Error (401 Unauthorized):**
*Occurs if the token is missing, expired, or invalid.*
```json
{
  "status": "error",
  "message": "Unauthorized access. Please login again."
}
```
---
## 3. Fetch Posted Jobs with Filter (GET)

Retrieves a list of available jobs. When a user selects a department from the dropdown menu.

*   **Endpoint:** `/postedJobs/{department}`
*   **Method:** `GET`
*   **Auth Required:** Yes (JWT)

#### **Query Parameters**
| Field | Type | Description |
| :--- | :--- | :--- |
| `department` | `string` | **(Optional)** When a user selects a department from the dropdown, it is passed as a parameter (e.g., Engineering, Design, HR). If no department is selected, all jobs are returned. 
|

#### **Request Headers**
| Field | Value | Description |
| :--- | :--- | :--- |
| `Authorization` | `Bearer <JWT_TOKEN>` | The token received from the Login API. |
| 
#### **Example Request URL**
`GET /jobs?department=Engineering`

---

#### **Response**

**Success (200 OK):**
```json
{
  "status": "success",
  "message": "Jobs retrieved successfully",
  "departmentSelected": "Engineering",
  "data": [
    {
      "jobId": "job_001",
      "title": "Backend Developer",
      "department": "Engineering",
      "applicants": "150",
      "shortlisted": "15",
      "postedDate": "2023-11-01"
    },
    {
      "jobId": "job_005",
      "title": "Frontend Engineer",
      "department": "Engineering",
      "applicants": "98",
      "shortlisted": "10",
      "postedDate": "2023-11-05"
    }
  ]
}
```

**Error (401 Unauthorized):**
Occurs if the token is missing, expired, or invalid.
```json
{
  "status": "error",
  "message": "Unauthorized access. Please login again."
}
```

**Error (404 Not Found):**
Occurs if no jobs exist for the selected department.
```json
{
  "status": "error",
  "message": "No jobs found for the selected department."
}
```
---

## 4. Search Jobs (GET)

This API allows users to search for available jobs based on keywords, location, and other filters.

*   **Endpoint:** `jobs/search`
*   **Method:** `GET`
*   **Auth Required:** Yes (JWT)

### Query Parameters

| Parameter  | Type   | Required | Description                                      |
| :--------- | :----- | :------- | :----------------------------------------------- |
| `q`        | String | No       | Search keywords (e.g., "Software Engineer")      |
| `location` | String | No       | City or remote preference (e.g., "London")       |
| `category` | String | No       | Job field (e.g., "IT", "Healthcare")             |
| `type`     | String | No       | Job type (e.g., "Full-time", "Contract")         |
| 

### Request Headers

| Field         | Type   | Required | Description                       |
| :------------ | :----- | :------- | :-------------------------------- |
| Authorization | String | Yes      | `Bearer <your_jwt_token>`         |

---

### Response

**Success (200 OK):**  
*Returns a list of jobs matching the search criteria.*

```json
{
  "status": "success",
  "totalResults": 2,
  "currentPage": 1,
  "jobs": [
    {
      "jobId": "JOB101",
      "title": "Frontend Developer",
      "company": "Tech Solutions",
      "location": "New York",
      "salary": "$80,000 - $120,000",
      "postedDate": "2023-10-25"
    },
    {
      "jobId": "JOB102",
      "title": "UI/UX Designer",
      "company": "Creative Agency",
      "location": "Remote",
      "salary": "Confidential",
      "postedDate": "2023-10-24"
    }
  ]
}
```

**No Results Found (200 OK):**  
*Successful request, but no jobs matched the criteria.*

```json
{
  "status": "success",
  "totalResults": 0,
  "jobs": [],
  "message": "No jobs found for the given criteria."
}
```

**Error (401 Unauthorized):**  
*Authentication failed.*

```json
{
  "status": "error",
  "message": "Unauthorized access. Please login again."
}
```

---



## 5. Candidates Dashboard Summary (GET)
Retrieves a simplified list of candidates for the main dashboard.

*   **Endpoint:** `/jobs/{jobId}/candidates-summary`
*   **Method:** `GET`
*   **Auth Required:** Yes (JWT)

### Query Parameters
| Parameter | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| `page` | Integer | No | 1 | The page number to retrieve (e.g., 1 for the first 10, 2 for the next 10). |
| `limit` | Integer | No | 10 | Number of candidates per page. |

### Response
*   **Success (200 OK):**
```json
{
  "status": "success",
  "metadata": {
    "totalCandidates": 45,
    "currentPage": 1,
    "totalPages": 5,
    "pageSize": 10,
    "hasNextPage": true,
    "hasPreviousPage": false
  },
  "data": [
    {
      "candidateId": "cand_001",
      "name": "John Smith",
      "currentRole": "Backend Developer",
      "companyName": "DevCorp",
      "yearOfExperience": 6,
      "aiScore": 95,
      "ctc": null
    },
    {
      "candidateId": "cand_002",
      "name": "Alice Wang",
      "currentRole": "Fullstack Engineer",
      "companyName": "TechSolutions",
      "yearOfExperience": 4,
      "aiScore": 92,
      "ctc": "null"
    },
    "... (8 more candidates)"
  ]
}
```
---
#### **Error Responses**

**401 Unauthorized** (Token is missing, invalid, or expired)
```json
{
  "status": "error",
  "message": "Unauthorized access. Please provide a valid token."
}
```

**500 Internal Server Error** (Server-side issue)
```json
{
  "status": "error",
  "message": "An internal error occurred while fetching candidates."
}
```

---

## 6. Job Post (GET)

This API verifies if the user and redirect  to the "Job Post" page.

*   **Endpoint:** `/job/Post`
*   **Method:** `GET`
*   **Auth Required:** Yes (JWT)

### Request Headers

| Field         | Type   | Required | Description                                      |
| :------------ | :----- | :------- | :----------------------------------------------- |
| Authorization | String | Yes      | ` <your_jwt_token>`                        |

---

### Response

**Success (200 OK):**  
*The token is valid. The frontend can now redirect the user to the "Job Post" page.*

```json
{
  "status": "success",
  "message": "TAccessing Job Post page...",
  "access": true
}
```

**Error (401 Unauthorized):**  
*The token is invalid or has expired. Redirect the user to the login page.*

```json
{
  "status": "error",
  "message": "Unauthorized access.",
  "access": false
}
```



---

## 5. Get Specific Job Post (GET)

This API verifies the user's authorization and provides the necessary data to redirect the user to a specific job details page using a `jobId`.

*   **Endpoint:** `/job/post/{jobId}`
*   **Method:** `GET`
*   **Auth Required:** Yes (JWT)

### Path Parameters

| Field | Type   | Required | Description                          |
| :---- | :----- | :------- | :----------------------------------- |
| jobId | String | Yes      | The unique identifier for the job post |

### Request Headers

| Field         | Type   | Required | Description                       |
| :------------ | :----- | :------- | :-------------------------------- |
| Authorization | String | Yes      | `Bearer <your_jwt_token>`         |

---

### Response

**Success (200 OK):**  
*Token is valid and the job exists. The frontend can now load the specific job page.*

```json
{
  "status": "success",
  "message": "Accessing Job Post details...",
  "access": true,
  "jobId": "JOB123456"
}
```

**Error (401 Unauthorized):**  
*Token is invalid or has expired. Redirect the user to the login page.*

```json
{
  "status": "error",
  "message": "Unauthorized access.",
  "access": false
}
```

**Error (404 Not Found):**  
*The specified Job ID does not exist in the database.*

```json
{
  "status": "error",
  "message": "Job Post not found.",
  "access": false
}
```

---

### 2. Upload Additional Resumes (POST)
This API allows the user to upload more resumes to an existing job. The backend will process these new resumes and **merge** the results with the existing applicant list.

*   **Endpoint:** `/job-requirements/{jobId}/resumes`
*   **Method:** `POST`
*   **Auth Required:** Yes (JWT)
*   **Content-Type:** `multipart/form-data`

#### **Request Body:**
| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `files` | Files[] | Yes | Additional PDF or Docx resume files. |

#### **Success Response (202 Accepted):**
```json
{
  "status": "success",
  "message": "Additional resumes uploaded successfully. Processing started.",
  "jobId": "job_123456",
  "newUploadCount": 50,
  "updatedTotalApplicants": 1000
}
```

---

### **Error Responses**

**404 Not Found** (If the Job ID is invalid)
```json
{
  "status": "error",
  "message": "The job requirement you are trying to access does not exist."
}
```

**403 Forbidden** (If the user tries to upload resumes to a job they did not create)
```json
{
  "status": "error",
  "message": "You do not have permission to modify this job post."
}
```
---


