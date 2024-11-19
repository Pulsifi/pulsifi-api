## **Pulsifi Platform API Integration Guideline v4.1.0 (BETA)**

## **Getting Started**

If you want to start with the Staging API environment, please request it from the Pulsifi account manager who manages the integration project.

## **Overview**

The Pulsifi API is built on REST principles, utilizing standard HTTP methods and response codes. All requests and responses are JSON-encoded.

### **Environments**

- **Production URL:** `https://api.pulsifi.me`
- **Staging URL:** `https://staging.api.pulsifi.me`

## **Authentication**

<details>
<summary><strong style="font-size: 1.3em;">Obtaining an Access Token</strong></summary>

#### **Pulsifi uses the OAuth2 Client Credentials Grant for authentication. A valid access token is required for all API calls.**

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

</details>

## **Job Endpoints**

<details>
<summary><strong style="font-size: 1.3em;">Create Job</strong></summary>

#### **Creates a new job in the Pulsifi system:**

- **Endpoint:** `POST /partner/v1.0/standard/jobs`
- **Request URL:** `https://api.pulsifi.me/partner/v1.0/standard/jobs`

#### **Headers:**

- `Accept: application/json`
- `Authorization: Bearer <access_token>`
- `Content-Type: application/json`

#### **Request Body Parameters:**

- **`title`**: The title of the job.

  - **Required**: Yes
  - **Type**: String
  - **Max Length**: 255 characters
  - **Nullable**: No

- **`description`**: A detailed description of the job.

  - **Required**: Yes
  - **Type**: String
  - **Min Length**: 300 characters
  - **Nullable**: No

- **`skills`**: Array of required skills for the job.

  - **Required**: Yes
  - **Type**: Array of Strings
  - **Max Items**: 10
  - **Nullable**: No

- **`external_id`**: A unique reference identifier for the external job ID.

  - **Required**: Yes
  - **Type**: String
  - **Max Length**: 45 characters
  - **Nullable**: No

#### **Example cURL:**

```bash
curl -X POST 'https://api.pulsifi.me/partner/v1.0/standard/jobs' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer <access_token>' \
  -H 'Content-Type: application/json' \
  -d '{
    "title": "Job Title",
    "description": "Job Description min 300 characters.",
    "skills": ["NodeJS", "Python", "PostgreSQL"],
    "external_id": "humanica_1001"
  }'
```

#### **Response Example:**

```json
{
  "id": "6a3d7ead-3d9d-4ee4-88f9-e6e59eaca51e",
  "title": "Job Title",
  "status": "active",
  "external_id": "humanica_1001",
  "employment_type": "fulltime",
  "description": "Job Description min 300 characters.",
  "skills": ["NodeJS", "Python", "PostgreSQL"]
}
```

#### **Response Body:**

- **`id`**: The unique identifier for the job created in the Pulsifi system.

  - **Type**: String (UUID)
  - **Nullable**: No

- **`title`**: The title of the job.

  - **Type**: String
  - **Nullable**: No

- **`status`**: The status of the job.

  - **Type**: Enum
  - **Default**: `active`
  - **Nullable**: No

- **`employment_type`**: The employment_type of the job.

  - **Type**: Enum
  - **Default**: `fulltime`
  - **Nullable**: No

- **`description`**: A detailed description of the job.

  - **Type**: String
  - **Nullable**: No

- **`skills`**: Array of required skills for the job.

  - **Type**: Array of Strings
  - **Nullable**: No

</details>
<details>
<summary><strong style="font-size: 1.3em;">Get Job Details</strong></summary>

#### **Use this endpoint to get job details:**

- **Endpoint:** `GET /partner/v1.0/standard/jobs/{job_id}`
- **Request URL:** `https://api.pulsifi.me/partner/v1.0/standard/jobs/{job_id}`

#### **Headers:**

- `Accept: application/json`
- `Authorization: Bearer <access_token>`
- `Content-Type: application/json`

#### **Path Parameter:**

- **`job_id`**: The unique identifier for the job created in the Pulsifi system.
  - **Required**: Yes
  - **Type**: String (UUID)
  - **Nullable**: No

#### **Example cURL:**

```bash
curl -X POST 'https://api.pulsifi.me/partner/v1.0/standard/jobs/{job_id}' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer <access_token>' \
  -H 'Content-Type: application/json' \
```

#### **Response Example:**

```json
{
  "id": "6a3d7ead-3d9d-4ee4-88f9-e6e59eaca51e",
  "title": "Job Title",
  "status": "active",
  "external_id": "pulsifi_1001",
  "employment_type": "fulltime",
  "description": "Job Description min 300 characters.",
  "skills": ["NodeJS", "Python", "PostgreSQL"]
}
```

#### **Response Body:**

- **`id`**: The unique identifier for the job created in the Pulsifi system.

  - **Type**: String (UUID)
  - **Nullable**: No

- **`title`**: The title of the job.

  - **Type**: String
  - **Nullable**: No

- **`status`**: The status of the job.

  - **Type**: Enum
  - **Default**: 'active'
  - **Nullable**: No

- **`employment_type`**: The employment_type of the job.

  - **Type**: Enum
  - **Default**: 'fulltime'
  - **Nullable**: No

- **`description`**: A detailed description of the job.

  - **Type**: String
  - **Nullable**: No

- **`skills`**: Array of required skills for the job.

  - **Type**: Array of Strings
  - **Nullable**: No

</details>

## **Candidate Endpoints**

<details>
<summary><strong style="font-size: 1.3em;">Create Candidate Invitation</strong></summary>

#### **Creates a new candidate invitation in the Pulsifi system:**

- **Endpoint:** `POST /partner/v1.0/standard/candidates`
- **Request URL:** `https://api.pulsifi.me/partner/v1.0/standard/candidates`

#### **Headers:**

- `Accept: application/json`
- `Authorization: Bearer <access_token>`
- `Content-Type: application/json`

#### **Request Body Parameters:**

- **`job_id`**: The unique identifier for the job created in the Pulsifi system. Refer to the [FAQ](#faq) below for details on how to obtain this ID.

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
  - **Default**: false
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
  - **Max Items**: 15
  - **Nullable**: No

  - Each skill object should contain:

    - **`name`**: The name of the skill.

      - **Required**: Yes
      - **Type**: String
      - **Max Length**: 255 characters
      - **Nullable**: No

    - **`proficiency`**: Proficiency level of the skill (e.g., `novice`, `beginner`,`competent`, `proficient`,`expert`).

      - **Required**: Yes
      - **Type**: Enum
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
  - **Default**: 3 months from the current date
  - **Nullable**: No
  - **Description**: Must be within 3 months from the current date. if not provided , the deadline will be defaulted to 3 months.

#### **Example cURL:**

```bash
curl -X POST 'https://api.pulsifi.me/partner/v1.0/standard/candidates' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer <access_token>' \
  -H 'Content-Type: application/json' \
  -d '{
    "job_id": "<Pulsifi Job ID>",
    "ext_reference_id": "<ATS Reference ID>",
    "is_anonymous_candidate": false,
    "email": "johndoe@gmail.com",
    "first_name": "John",
    "last_name": "Doe",
    "deadline": "2021-08-12T12:21:59Z",
    "skills": [
      {
        "name": "Programming",
        "proficiency": "novice"
      }
    ],
    "work_experiences": [
      {
        "organization": "Pulsifi",
        "role": "Software Engineer",
        "is_current": true,
        "start_date": "2022-10-04",
        "end_date": "2024-10-04",
        "responsibilities_achievements": "Developing APIs for internal products and services."
      }
    ]
  }'
```

#### **Response Example:**

```json
{
  "is_anonymous_candidate": false,
  "email": "johndoe@gmail.com",
  "first_name": "John",
  "last_name": "Doe",
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

- **`job_id`**: The unique identifier for the job created in the Pulsifi system.

  - **Type**: String (UUID)
  - **Nullable**: No

- **`is_anonymous_candidate`**: Indicates whether the candidate's details should be anonymous.

  - **Type**: Boolean
  - **Nullable**: Yes

- **`email`**: The candidate's email address.

  - **Type**: String
  - **Nullable**: Yes

- **`first_name`**: The candidate's first name.

  - **Type**: String
  - **Nullable**: Yes

- **`last_name`**: The candidate's last name.

  - **Type**: String
  - **Nullable**: Yes

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

</details>
<details>
<summary><strong style="font-size: 1.3em;">Get Candidate Result / Details</strong></summary>

#### **Use this endpoint to get candidate results or details.**

- **Endpoint:** `GET /partner/v1.0/standard/candidates/{candidate_id}`
- **Request URL:** `https://api.pulsifi.me/partner/v1.0/standard/candidates/{candidate_id}`

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
  curl -X GET 'https://api.pulsifi.me/partner/v1.0/standard/candidates/{candidate_id}' \
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
  "is_anonymous_candidate": false,
  "email": "johndoe@gmail.com",
  "first_name": "John",
  "last_name": "Doe",
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
    },
    {
      "score_format": 100,
      "score_type": "reasoning_verbal",
      "score_value": 88
    },
    {
      "score_format": 100,
      "score_type": "reasoning_logical",
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

- **`is_anonymous_candidate`**: Indicates whether the candidate's details should be anonymous.

  - **Type**: Boolean
  - **Nullable**: Yes

- **`email`**: The candidate's email address.

  - **Type**: String
  - **Nullable**: Yes

- **`first_name`**: The candidate's first name.

  - **Type**: String
  - **Nullable**: Yes

- **`last_name`**: The candidate's last name.

  - **Type**: String
  - **Nullable**: Yes

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

    - **`score_type`**: The type of the score (e.g., `hard_skill`, `work_experience`,`work_interest`,`work_style`, `work_value`,`reasoning_average`, `reasoning_logical`, `reasoning_numeric`, `reasoning_verbal`).

      - **Type**: String
      - **Nullable**: No

    - **`score_value`**: The value of the score.
      - **Type**: Integer
      - **Max Value**: 100
      - **Nullable**: No

</details>
<details>
<summary><strong style="font-size: 1.3em;">Get Candidate Result / Details With Invitation Code</strong></summary>

#### **Use this endpoint to get candidate results or details.**

- **Endpoint:** `GET /partner/v1.0/standard/candidates/invitation/{invite_code}`
- **Request URL:** `https://api.pulsifi.me/partner/v1.0/standard/candidates/invitation/{invite_code}`

#### **Headers:**

- `Accept: application/json`
- `Authorization: Bearer <access_token>`

#### **Path Parameter:**

- **`invite_code`**: Another unique identifier for the candidate in the Pulsifi system.
  - **Required**: Yes
  - **Type**: String
  - **Max Length**: Varies
  - **Nullable**: No

#### **Example cURL:**

```bash
  curl -X GET 'https://api.pulsifi.me/partner/v1.0/standard/candidates/invitation/{invite_code}' \
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
  "is_anonymous_candidate": false,
  "email": "johndoe@gmail.com",
  "first_name": "John",
  "last_name": "Doe",
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
    },
    {
      "score_format": 100,
      "score_type": "reasoning_verbal",
      "score_value": 88
    },
    {
      "score_format": 100,
      "score_type": "reasoning_logical",
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

- **`is_anonymous_candidate`**: Indicates whether the candidate's details should be anonymous.

  - **Type**: Boolean
  - **Nullable**: Yes

- **`email`**: The candidate's email address.

  - **Type**: String
  - **Nullable**: Yes

- **`first_name`**: The candidate's first name.

  - **Type**: String
  - **Nullable**: Yes

- **`last_name`**: The candidate's last name.

  - **Type**: String
  - **Nullable**: Yes

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

    - **`score_type`**: The type of the score (e.g., `hard_skill`, `work_experience`,`work_interest`,`work_style`, `work_value`,`reasoning_average`, `reasoning_logical`, `reasoning_numeric`, `reasoning_verbal`).

      - **Type**: String
      - **Nullable**: No

    - **`score_value`**: The value of the score.
      - **Type**: Integer
      - **Max Value**: 100
      - **Nullable**: No

</details>

## **Webhook Endpoints**

<details>
<summary><strong style="font-size: 1.3em;">Create Webhook</strong></summary>

#### **Creates a new webhook in the Pulsifi system:**

- **Endpoint:** `POST /partner/v1.0/webhooks`
- **Request URL:** `https://api.pulsifi.me/partner/v1.0/webhooks`

#### **Headers:**

- `Accept: application/json`
- `Authorization: Bearer <access_token>`
- `Content-Type: application/json`

#### **Request Body Parameters:**

- **`name`**: The name of the webhook.

  - **Required**: Yes
  - **Type**: String
  - **Max Length**: 255 characters
  - **Nullable**: No

- **`url`**: The URL to which the webhook will send data.

  - **Required**: Yes
  - **Type**: String (URL)
  - **Max Length**: 2048 characters
  - **Nullable**: No

- **`events`**: The events that trigger the webhook (e.g., `candidate_application_result_ready`, `candidate_job_recommendation_result_ready`).

  - **Required**: Yes
  - **Type**: Array of Enum values

- **`secret`**: The secret in the webhook headers, ensuring verification.

  - **Required**: No
  - **Type**: String
  - **Max Length**: 255 characters

#### **Example cURL:**

```bash
  curl -X POST 'https://api.pulsifi.me/partner/v1.0/webhooks' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <access_token>' \
  -d '{
    "name": "Webhook 1",
    "url": "https://example.com/my/webhook/endpoint",
    "events": [
      "candidate_application_result_ready"
    ],
    "secret": "string"
  }'
```

#### **Response Example:**

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "partner_id": 1,
  "name": "Webhook 1",
  "url": "https://example.com/my/webhook/endpoint",
  "events": ["candidate_application_result_ready"],
  "status": "active",
  "is_deleted": false,
  "created_at": "2024-10-04T04:06:55.124Z",
  "created_by": 1
}
```

#### **Response Body:**

- **`id`**: The unique identifier for the webhook created in the Pulsifi system.

  - **Type**: String (UUID)
  - **Nullable**: No

- **`partner_id`**: The unique identifier for partners in the Pulsifi system.

  - **Type**: Number
  - **Nullable**: No

- **`name`**: The name of the webhook.

  - **Type**: String
  - **Nullable**: No

- **`url`**: The URL to which the webhook will send data.

  - **Type**: String (URL)
  - **Nullable**: No

- **`events`**: The events that trigger the webhook.

  - **Type**: Array of Enum values
  - **Nullable**: No

- **`status`**: The status of the webhook (e.g., `active`, `inactive`).

  - **Type**: Enum
  - **Nullable**: No

- **`is_deleted`**: Indicates whether the webhook is deleted.

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`created_at`**: The date and time when the webhook was created (in UTC format).

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`created_by`**: The unique identifier for partners in the Pulsifi system.

  - **Type**: Number
  - **Nullable**: No

</details>
<details>
<summary><strong style="font-size: 1.3em;">Update Webhook</strong></summary>

#### **Use this endpoint to update webhook in the Pulsifi system.**

- **Endpoint:** `PUT /partner/v1.0/webhooks/{webhook_id}`
- **Request URL:** `https://api.pulsifi.me/partner/v1.0/webhooks/{webhook_id}`

#### **Headers:**

- `Accept: application/json`
- `Authorization: Bearer <access_token>`

#### **Request Body Parameters:**

- **`name`**: The name of the webhook.

  - **Required**: Yes
  - **Type**: String
  - **Max Length**: 255 characters
  - **Nullable**: No

- **`url`**: The URL to which the webhook will send data.

  - **Required**: Yes
  - **Type**: String (URL)
  - **Max Length**: 2048 characters
  - **Nullable**: No

- **`events`**: The events that trigger the webhook (e.g., `candidate_application_result_ready`, `candidate_job_recommendation_result_ready`).

  - **Required**: Yes
  - **Type**: Array of Enum values

- **`secret`**: The secret in the webhook headers, ensuring verification.

  - **Required**: No
  - **Type**: String
  - **Max Length**: 255 characters

#### **Path Parameter:**

- **`webhook_id`**: The unique identifier for the webhook created in the Pulsifi system.
  - **Required**: Yes
  - **Type**: String (UUID)
  - **Nullable**: No

#### **Example cURL:**

```bash
  curl -X PUT 'https://api.pulsifi.me/partner/v1.0/webhooks/{webhook_id}' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <access_token>' \
  -d '{
    "name": "Webhook 1",
    "url": "https://example.com/my/webhook/endpoint",
    "events": [
      "candidate_application_result_ready"
    ],
    "secret": "string"
  }'
```

#### **Response Example:**

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "partner_id": 1,
  "name": "Webhook 1",
  "url": "https://example.com/my/webhook/endpoint",
  "events": ["candidate_application_result_ready"],
  "status": "active",
  "is_deleted": false,
  "created_at": "2024-10-04T04:06:55.124Z",
  "created_by": 1
}
```

#### **Response Body:**

- **`id`**: The unique identifier for the webhook created in the Pulsifi system.

  - **Type**: String (UUID)
  - **Nullable**: No

- **`partner_id`**: The unique identifier for partners in the Pulsifi system.

  - **Type**: Number
  - **Nullable**: No

- **`name`**: The name of the webhook.

  - **Type**: String
  - **Nullable**: No

- **`url`**: The URL to which the webhook will send data.

  - **Type**: String (URL)
  - **Nullable**: No

- **`events`**: The events that trigger the webhook.

  - **Type**: Array of Enum values
  - **Nullable**: No

- **`status`**: The status of the webhook (e.g., `active`, `inactive`).

  - **Type**: Enum
  - **Nullable**: No

- **`is_deleted`**: Indicates whether the webhook is deleted.

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`created_at`**: The date and time when the webhook was created (in UTC format).

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`created_by`**: The unique identifier for partners in the Pulsifi system.

  - **Type**: Number
  - **Nullable**: No

</details>
<details>
<summary><strong style="font-size: 1.3em;">Delete Webhook</strong></summary>

#### **Use this endpoint to delete webhook in the Pulsifi system.**

- **Endpoint:** `DELETE /partner/v1.0/webhooks/{webhook_id}`
- **Request URL:** `https://api.pulsifi.me/partner/v1.0/webhooks/{webhook_id}`

#### **Headers:**

- `Accept: application/json`
- `Authorization: Bearer <access_token>`

#### **Path Parameter:**

- **`webhook_id`**: The unique identifier for the webhook created in the Pulsifi system.
  - **Required**: Yes
  - **Type**: String (UUID)
  - **Nullable**: No

#### **Example cURL:**

```bash
  curl -X DELETE 'https://api.pulsifi.me/partner/v1.0/webhooks/{webhook_id}' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <access_token>' \
```

#### **Response Example:**

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "partner_id": 1,
  "name": "Webhook 1",
  "url": "https://example.com/my/webhook/endpoint",
  "events": ["candidate_application_result_ready"],
  "status": "active",
  "is_deleted": true,
  "created_at": "2024-10-04T04:06:55.124Z",
  "created_by": 1
}
```

#### **Response Body:**

- **`id`**: The unique identifier for the webhook created in the Pulsifi system.

  - **Type**: String (UUID)
  - **Nullable**: No

- **`partner_id`**: The unique identifier for partners in the Pulsifi system.

  - **Type**: Number
  - **Nullable**: No

- **`name`**: The name of the webhook.

  - **Type**: String
  - **Nullable**: No

- **`url`**: The URL to which the webhook will send data.

  - **Type**: String (URL)
  - **Nullable**: No

- **`events`**: The events that trigger the webhook.

  - **Type**: Array of Enum values
  - **Nullable**: No

- **`status`**: The status of the webhook (e.g., `active`, `inactive`).

  - **Type**: Enum
  - **Nullable**: No

- **`is_deleted`**: Indicates whether the webhook is deleted.

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`created_at`**: The date and time when the webhook was created (in UTC format).

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`created_by`**: The unique identifier for partners in the Pulsifi system.

  - **Type**: Number
  - **Nullable**: No

</details>

<details>
<summary><strong style="font-size: 1.3em;">Get All Webhook Details</strong></summary>

#### **Use this endpoint to get one webhook details.**

- **Endpoint:** `GET /partner/v1.0/webhooks`
- **Request URL:** `https://api.pulsifi.me/partner/v1.0/webhooks`

#### **Headers:**

- `Accept: application/json`
- `Authorization: Bearer <access_token>`

#### **Query Parameter:**

?page=1&page_size=25&sort_by=created_at&status=active&q=Webhook%201'

- **`page`**: Indicates which specific page of results you want to retrieve.

  - **Required**: No
  - **Default**: 1
  - **Type**: Number
  - **Nullable**: No

- **`page_size`**: Indicates how many items should be included in each page of results.

  - **Required**: No
  - **Type**: Number
  - **Default**: 25
  - **Nullable**: No

  - **`sort_by`**: Comma separated sortable fields (e.g., `created_at`, `+created_at`, `-created_at`, `name`, `+name`, `-name`).
  - **Required**: No
  - **Type**: String
  - **Nullable**: No

  - **`status`**: The status of the webhook (e.g., `active`, `inactive`).
  - **Required**: No
  - **Type**: String
  - **Nullable**: No

  - **`q`**: Keyword to filter with webhook name.
  - **Required**: No
  - **Type**: String
  - **Nullable**: No

#### **Example cURL:**

```bash
  curl -X GET 'https://api.pulsifi.me/partner/v1.0/webhooks/?page=1&page_size=25&sort_by=created_at&status=active&q=Webhook%201' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <access_token>' \
```

#### **Response Example:**

```json
{
  "total_count": 1,
  "result": [
    {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "partner_id": 1,
      "name": "Webhook 1",
      "url": "https://example.com/my/webhook/endpoint",
      "events": ["candidate_application_result_ready"],
      "status": "active",
      "is_deleted": false,
      "created_at": "2024-10-04T04:06:55.124Z",
      "created_by": 1
    }
  ]
}
```

#### **Response Body:**

- **`id`**: The unique identifier for the webhook created in the Pulsifi system.

  - **Type**: String (UUID)
  - **Nullable**: No

- **`partner_id`**: The unique identifier for partners in the Pulsifi system.

  - **Type**: Number
  - **Nullable**: No

- **`name`**: The name of the webhook.

  - **Type**: String
  - **Nullable**: No

- **`url`**: The URL to which the webhook will send data.

  - **Type**: String (URL)
  - **Nullable**: No

- **`events`**: The events that trigger the webhook.

  - **Type**: Array of Enum values
  - **Nullable**: No

- **`status`**: The status of the webhook (e.g., `active`, `inactive`).

  - **Type**: Enum
  - **Nullable**: No

- **`is_deleted`**: Indicates whether the webhook is deleted.

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`created_at`**: The date and time when the webhook was created (in UTC format).

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`created_by`**: The unique identifier for partners in the Pulsifi system.

  - **Type**: Number
  - **Nullable**: No

</details>

<details>
<summary><strong style="font-size: 1.3em;">Get Webhook Details</strong></summary>

#### **Use this endpoint to get one webhook details.**

- **Endpoint:** `GET /partner/v1.0/webhooks/{webhook_id}`
- **Request URL:** `https://api.pulsifi.me/partner/v1.0/webhooks/{webhook_id}`

#### **Headers:**

- `Accept: application/json`
- `Authorization: Bearer <access_token>`

#### **Path Parameter:**

- **`webhook_id`**: The unique identifier for the webhook created in the Pulsifi system.
  - **Required**: Yes
  - **Type**: String (UUID)
  - **Nullable**: No

#### **Example cURL:**

```bash
  curl -X GET 'https://api.pulsifi.me/partner/v1.0/webhooks/{webhook_id}' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <access_token>' \
```

#### **Response Example:**

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "partner_id": 1,
  "name": "Webhook 1",
  "url": "https://example.com/my/webhook/endpoint",
  "events": ["candidate_application_result_ready"],
  "status": "active",
  "is_deleted": false,
  "created_at": "2024-10-04T04:06:55.124Z",
  "created_by": 1
}
```

#### **Response Body:**

- **`id`**: The unique identifier for the webhook created in the Pulsifi system.

  - **Type**: String (UUID)
  - **Nullable**: No

- **`partner_id`**: The unique identifier for partners in the Pulsifi system.

  - **Type**: Number
  - **Nullable**: No

- **`name`**: The name of the webhook.

  - **Type**: String
  - **Nullable**: No

- **`url`**: The URL to which the webhook will send data.

  - **Type**: String (URL)
  - **Nullable**: No

- **`events`**: The events that trigger the webhook.

  - **Type**: Array of Enum values
  - **Nullable**: No

- **`status`**: The status of the webhook (e.g., `active`, `inactive`).

  - **Type**: Enum
  - **Nullable**: No

- **`is_deleted`**: Indicates whether the webhook is deleted.

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`created_at`**: The date and time when the webhook was created (in UTC format).

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`created_by`**: The unique identifier for partners in the Pulsifi system.

  - **Type**: Number
  - **Nullable**: No

</details>

<details>
<summary><strong style="font-size: 1.3em;">Update Webhook Status</strong></summary>

#### **Use this endpoint to update webhook status in the Pulsifi system.**

- **Endpoint:** `PUT /partner/v1.0/webhooks/{webhook_id}/status`
- **Request URL:** `https://api.pulsifi.me/partner/v1.0/webhooks/{webhook_id}/status`

#### **Headers:**

- `Accept: application/json`
- `Authorization: Bearer <access_token>`

#### **Path Parameter:**

- **`webhook_id`**: The unique identifier for the webhook created in the Pulsifi system.
  - **Required**: Yes
  - **Type**: String (UUID)
  - **Nullable**: No

#### **Request Body Parameters:**

- **`status`**: The status of the webhook (e.g., `active`, `inactive`).

  - **Required**: Yes
  - **Type**: Enum
  - **Nullable**: No

#### **Example cURL:**

```bash
  curl -X PUT 'https://api.pulsifi.me/partner/v1.0/webhooks/{webhook_id}/status' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <access_token>' \
  -d '{
    "status": "inactive"
  }'
```

#### **Response Example:**

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "partner_id": 1,
  "name": "Webhook 1",
  "url": "https://example.com/my/webhook/endpoint",
  "events": ["candidate_application_result_ready"],
  "status": "inactive",
  "is_deleted": false,
  "created_at": "2024-10-04T04:06:55.124Z",
  "created_by": 1
}
```

#### **Response Body:**

- **`id`**: The unique identifier for the webhook created in the Pulsifi system.

  - **Type**: String (UUID)
  - **Nullable**: No

- **`partner_id`**: The unique identifier for partners in the Pulsifi system.

  - **Type**: Number
  - **Nullable**: No

- **`name`**: The name of the webhook.

  - **Type**: String
  - **Nullable**: No

- **`url`**: The URL to which the webhook will send data.

  - **Type**: String (URL)
  - **Nullable**: No

- **`events`**: The events that trigger the webhook.

  - **Type**: Array of Enum values
  - **Nullable**: No

- **`status`**: The status of the webhook (e.g., `active`, `inactive`).

  - **Type**: Enum
  - **Nullable**: No

- **`is_deleted`**: Indicates whether the webhook is deleted.

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`created_at`**: The date and time when the webhook was created (in UTC format).

  - **Type**: String (UTC Date)
  - **Nullable**: No

- **`created_by`**: The unique identifier for partners in the Pulsifi system.

  - **Type**: Number
  - **Nullable**: No

</details>

<details>
<summary><strong style="font-size: 1.3em;">Test Webhook</strong></summary>

#### **Use this endpoint to test webhook.**

- **Endpoint:** `POST /partner/v1.0/webhooks/test`
- **Request URL:** `https://api.pulsifi.me/partner/v1.0/webhooks/test`

#### **Headers:**

- `Accept: application/json`
- `Authorization: Bearer <access_token>`

#### **Request Body Parameters:**

- **`event`**: The event that trigger the webhook (e.g., `candidate_application_result_ready`, `candidate_job_recommendation_result_ready`).

  - **Required**: Yes
  - **Type**: Enum
  - **Nullable**: No

#### **Example cURL:**

```bash
  curl -X PUT 'https://api.pulsifi.me/partner/v1.0/webhooks/test' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <access_token>' \
  -d '{
    "event": "candidate_application_result_ready"
  }'
```

</details>

## **FAQ**

### **How do I encode client credentials?**

Use Base64 encoding for your client ID and client secret in the format `client_id:client_secret`. Tools like [base64encode.org](https://www.base64encode.org/) can help with this.

### **How do I handle expired tokens?**

Tokens expire after the time specified in `expires_in`. Obtain a new token using the authentication endpoint when the current one expires.

---
