## **Pulsifi Platform API Integration Guideline v3.0.0 (BETA)**

### **Overview**

The Pulsifi API is built on REST principles, utilizing standard HTTP methods and response codes. All requests and responses are JSON-encoded.

### **Environments**

- **Production URL:** `https://api.pulsifi.me`
- **Staging URL:** `https://staging.api.pulsifi.me`

## **Authentication**

### **Obtaining an Access Token**

Pulsifi uses the OAuth2 Client Credentials Grant for authentication. A valid access token is required for all API calls.

- **Endpoint:** `POST /partner/oauth2/token`
- **Request URL:** `https://api.pulsifi.me/partner/oauth2/token`

#### **Headers:**

- `Accept: application/json`
- `Authorization: Basic <YOUR-ENCODED-CLIENT-ID-CLIENT-SECRET>`
- `Content-Type: application/json`

#### **Request Body:**

- **`grant_type`**: The type of OAuth2 grant.
  - **Required**: Yes
  - **Value**: Must be `"client_credentials"`
  - **Nullable**: No

#### **Example cURL:**

```bash
curl --request POST 'https://api.pulsifi.me/partner/oauth2/token' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic YOUR-ENCODED-CLIENT-ID-CLIENT-SECRET' \
  --header 'Content-Type: application/json' \
  --data-raw '{"grant_type": "client_credentials"}'
```

#### **Response Example:**

```json
{
  "access_token": "string",
  "scope": "string",
  "expires_in": "string",
  "token_type": "bearer"
}
```

#### **Response Body:**

- **`access_token`**: The token that must be included in the `Authorization` header of all subsequent API requests.

  - **Type**: String
  - **Nullable**: No

- **`scope`**: The permissions granted to the access token, defining what actions or resources it can access.

  - **Type**: String
  - **Nullable**: No

- **`expires_in`**: The lifetime of the access token in seconds.

  - **Type**: String
  - **Nullable**: No

- **`token_type`**: The type of the token, usually `bearer`, indicating how the token should be included in API requests.

  - **Type**: String
  - **Nullable**: No

---

## **Endpoints**

### **1. Create Candidate Invitation**

Creates a new candidate invitation in the Pulsifi system.

- **Endpoint:** `POST /partner/v1.0/standard/candidate`
- **Request URL:** `https://api.pulsifi.me/partner/v1.0/standard/candidate`

#### **Headers:**

- `Accept: application/json`
- `Authorization: Bearer <access_token>`
- `Content-Type: application/json`

#### **Request Body Parameters:**

- **`job_id`**: The unique identifier for the job in the Pulsifi system. Refer to the [FAQ](#faq) below for details on how to obtain this ID.

  - **Required**: Yes
  - **Type**: String (UUID)
  - **Max Length**: 36 characters
  - **Nullable**: No

- **`ext_reference_id`**: External job application ID from the Applicant Tracking System (ATS).

  - **Required**: Yes
  - **Type**: String
  - **Max Length**: 50 characters
  - **Nullable**: No

- **`is_anonymous_candidate`**: Indicates whether the candidate's details should be anonymous.

  - **Required**: Yes
  - **Type**: Boolean
  - **Nullable**: Yes
  - **Behavior**: If set to `true`, the `email`, `first_name`, and `last_name` fields become optional.

- **`email`**: The candidate's email address.

  - **Required**: No (if `is_anonymous_candidate` is `true`)
  - **Type**: String
  - **Max Length**: 255 characters
  - **Nullable**: Yes

- **`first_name`**: The candidate's first name.

  - **Required**: No (if `is_anonymous_candidate` is `true`)
  - **Type**: String
  - **Max Length**: 255 characters
  - **Nullable**: Yes

- **`last_name`**: The candidate's last name.

  - **Required**: No (if `is_anonymous_candidate` is `true`)
  - **Type**: String
  - **Max Length**: 255 characters
  - **Nullable**: Yes

- **`skills`**: A list of skills possessed by the candidate.

  - **Required**: No
  - **Type**: Array of Strings
  - **Max Items**: 20
  - **Nullable**: No

- **`work_experiences`**: A list of the candidate's work experiences.

  - **Required**: No
  - **Type**: Array of Objects
  - **Max Items**: 10
  - **Nullable**: No

  - Each work experience object should contain:

    - **`role`**: Role title of the candidate in the organization.

      - **Required**: Yes
      - **Type**: String
      - **Max Length**: 255 characters
      - **Nullable**: No

    - **`organization`**: Name of the organization.

      - **Required**: Yes
      - **Type**: String
      - **Max Length**: 255 characters
      - **Nullable**: No

    - **`is_current`**: Indicates whether this is the candidate’s current role.

      - **Required**: Yes
      - **Type**: Boolean
      - **Nullable**: No

    - **`responsibility_achievement`**: Responsibilities and achievements of the candidate in the organization.

      - **Required**: No
      - **Type**: String
      - **Max Length**: 1000 characters
      - **Nullable**: Yes

    - **`start_date`**: Start date of the work experience.

      - **Required**: Yes
      - **Type**: String (YYYY-MM-DD)
      - **Max Length**: 10 characters
      - **Nullable**: No

    - **`end_date`**: End date of the work experience.
      - **Required**: No
      - **Type**: String (YYYY-MM-DD)
      - **Max Length**: 10 characters
      - **Nullable**: Yes

- **`deadline`**: The assessment invitation deadline.
  - **Required**: No
  - **Type**: String (UTC Date)
  - **Nullable**: No
  - **Description**: Must be within 3 months from the current date.

#### **Example cURL:**

```bash
curl -X POST 'https://api.pulsifi.me/partner/v1.0/standard/candidate' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer <access_token>' \
  -H 'Content-Type: application/json' \
  -d '{
    "job_id": "<Pulsifi Job ID>",
    "ext_reference_id": "<ATS Reference ID>",
    "email": "johndoe@gmail.com",
    "first_name": "John",
    "last_name": "Doe",
    "deadline": "2021-08-12T12:21:59Z"
  }'
```

#### **Response Example:**

```json
{
  "status": "invited",
  "ext_reference_id": "string",
  "job_id": "uuid",
  "candidate_id": "string",
  "invitation_link": "string",
  "invitation_expired_at": "2024-12-31T23:59:59Z",
  "created_at": "2024-09-02T10:30:00Z"
}
```

#### **Response Body:**

- **`status`**: The current status of the candidate's invitation, starting with the initial state of “invited.”

  - **Type**: String
  - **Nullable**: No

- **`ext_reference_id`**: The job application ID provided by an external ATS.

  - **Type**: String
  - **Nullable**: No

- **`job_id`**: The unique identifier for the job in the Pulsifi system.

  - **Type**: String (UUID)
  - **Nullable**: No

- **`candidate_id`**: The unique identifier for the candidate in the Pulsifi system. This value should be stored in your database for future queries about the candidate's assessment status and results.

  - **Type**: String
  - **Nullable**: No

- **`invitation_link`**: A URL that the candidate can use to access the Pulsifi assessment.

  - **Type**: String (URL)
  - **Nullable**: No

- **`invitation_expired_at`**: The expiration date and time of the invitation link (in UTC format).

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`created_at`**: The date and time when the invitation was created (in UTC format).
  - **Type**: String (UTC Date)
  - **Nullable**: No

---

### **2. Get Candidate Result / Details**

Use this endpoint to get candidate results or details.

- **Endpoint:** `GET /partner/v1.0/standard/candidate/{candidate_id}`
- **Request URL:** `https://api.pulsifi.me/partner/v1.0/standard/candidate/{candidate_id}`

#### **Headers:**

- `Accept: application/json`
- `Authorization: Bearer <access_token>`

#### **Path Parameter:**

- **`candidate_id`**: The unique identifier for the candidate in the Pulsifi system.
  - **Required**: Yes
  - **Type**: String
  - **Max Length**: Varies
  - **Nullable**: No

#### **Example cURL:**

```bash
  curl -X GET 'https://api.pulsifi.me/partner/v1.0/standard/candidate/{candidate_id}/details' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer <access_token>' \
  -H 'Content-Type: application/json'
```

#### **Response Example:**

```json
{
  "status": "completed",
  "ext_reference_id": "ATS12345",
  "job_id": "uuid",
  "candidate_id": "string",
  "invitation_link": "string",
  "invitation_expired_at": "2024-12-31T23:59:59Z",
  "created_at": "2024-09-02T10:30:00Z",
  "report_pdf_link": "https://link.to/pdf",
  "report_profile_link": "https://link.to/profile",
  "scores": [
    {
      "score_format": 100,
      "score_type": "role_fit",
      "score_value": 85
    },
    {
      "score_format": 100,
      "score_type": "organizational_fit",
      "score_value": 90
    }
  ],
  "additional_scores": [
    {
      "score_format": 100,
      "score_type": "reasoning_numeric",
      "score_value": 88
    }
  ]
}
```

#### **Response Body:**

- **`status`**: The current status of the candidate's assessment (e.g., `invited`, `expired`, `opened`, `started`, `completed`).

  - **Type**: String
  - **Nullable**: No

- **`ext_reference_id`**: The external job application ID from the ATS.

  - **Type**: String
  - **Nullable**: No

- **`job_id`**: The unique job identifier in the Pulsifi system.

  - **Type**: String (UUID)
  - **Nullable**: No

- **`candidate_id`**: The unique candidate identifier in the Pulsifi system.

  - **Type**: String
  - **Nullable**: No

- **`invitation_link`**: URL to the candidate's assessment invitation.

  - **Type**: String (URL)
  - **Nullable**: No

- **`invitation_expired_at`**: The expiration date and time of the invitation link (UTC format).

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`created_at`**: The date and time when the invitation was created (UTC format).

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`report_pdf_link`**: URL to download the candidate's assessment report in PDF format. The link is valid for up to 3 months.

  - **Type**: String (URL)
  - **Nullable**: Yes

- **`report_profile_link`**: URL to view the candidate's assessment profile.

  - **Type**: String (URL)
  - **Nullable**: Yes

- **`scores`**: A list of Pulsifi fit scores objects representing different aspects of the candidate's assessment.

  - **Type**: Array of Objects
  - **Nullable**: Yes

  - Each score object should contain:

    - **`score_format`**: The format of the score (e.g., 100 for percentage).

      - **Type**: Integer
      - **Nullable**: No

    - **`score_type`**: The type of the score (e.g., `role_fit`, `organizational_fit`).

      - **Type**: String
      - **Nullable**: No

    - **`score_value`**: The value of the score.
      - **Type**: Integer
      - **Max Value**: 100
      - **Nullable**: No

- **`additional_scores`**: A list of additional score objects that contribute to actual Pulsifi fit scores.

  - **Type**: Array of Objects
  - **Nullable**: Yes

  - Each additional score object should contain:

    - **`score_format`**: The format of the score (e.g., 100 for percentage).

      - **Type**: Integer
      - **Nullable**: No

    - **`score_type`**: The type of the score (e.g., `work_value`, `work_style`,`work_experience`,`reasoning_verbal`, `reasoning_logical`,`reasoning_numeric`, `interest_riasec`, `hard_skills`).

      - **Type**: String
      - **Nullable**: No

    - **`score_value`**: The value of the score.
      - **Type**: Integer
      - **Max Value**: 100
      - **Nullable**: No

---

## **FAQ**

### **What is `job_id`?**

- You can obtain the `job_id` by accessing the job module in the Pulsifi app:
  - Select the job you want the candidate to apply to and view the job details.
  - The job ID can be found in the browser URL, for example: *https://app.pulsifi.me/acquisition/jobs/{job_id}*.
- If you are a third-party ATS platform without access to the Pulsifi app, please request it from the Pulsifi account manager who manages the integration project.

- If you want to start with the Staging API environment, please request it from the Pulsifi account manager who manages the integration project.

- If you are a partner, please use the "Publish Job" endpoint to obtain a job ID.

### **How do I encode client credentials?**

Use Base64 encoding for your client ID and client secret in the format `client_id:client_secret`. Tools like [base64encode.org](https://www.base64encode.org/) can help with this.

### **How do I handle expired tokens?**

Tokens expire after the time specified in `expires_in`. Obtain a new token using the authentication endpoint when the current one expires.

---
