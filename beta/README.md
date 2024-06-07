# Pulsifi Platform API Integration Guideline v2.0.0 (BETA)

## Introduction
<br />

The Pulsifi API is organized around REST. Our API accepts json-encoded request bodies, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

We offer <strong>Staging Environment API</strong> (Refer to FAQ section below) and you can start using by adding prefix of <strong>"staging."</strong> in Pulsifi API url


#### Example

```
curl --request POST \
'https://staging.api.pulsifi.me/partner/oauth2/token' \
  -H 'accept: application/json' \
  -H 'Authorization: Basic YOUR-ENCODED-CLIENT-ID-CLIENT-SECRET' \
  -H 'Content-Type: application/json' \
  -d '{
  "grant_type": "client_credentials"
}'
```

## Authentication : OAuth2 Client Credentials Grant

<br />

Pulsifi's Integration Team will provide the necessary **client ID/client secret**

Pulsifi integration API calls will be authenticated using **OAuth2 Client Credentials Grant**.

<b>Prerequisite:</b> Client need get a valid access token to call the integration API's with the provided client id and secret.

<br />

The following is a sample of OAuth2 Client Credentials Grant credentials:

```
CLIENT-ID: <will be provided by Pulsifi>
CLIENT-SECRET: <will be provided by Pulsifi>
```

The following is a sample of a Post request to get access token:

```
curl --request POST \
'https://api.pulsifi.me/partner/oauth2/token' \
  -H 'accept: application/json' \
  -H 'Authorization: Basic YOUR-ENCODED-CLIENT-ID-CLIENT-SECRET' \
  -H 'Content-Type: application/json' \
  -d '{
  "grant_type": "client_credentials"
}'
```

#### Response

<br />
  <table border=1>
  <tr>
  <th>Attribute</th>
  <th>Type</th>
  <th>Description</th>
  </tr>

  <tr>
  <td>access_token</td>
  <td>String</td>
  <td>Valid access token will be generated</td>
  </tr>

  <tr>
  <td>scope</td>
  <td>String</td>
  <td>Default scope</td>
  </tr>

  <tr>
  <td>expires_in</td>
  <td>Number</td>
  <td>Access token expiry datetime in unixtimestamp</td>
  </tr>

  <tr valign=top>
  <td>token_type</td>
  <td>String</td>
  <td><b>Default</b> : Bearer
  </td>
  </tr>
  </table>
<br /><br />

## Retrieve Candidate assessment result

If the assessment result data need to capture and store in your ATS platform,

you can either get assessment result data based on HTTP GET https://api.pulsifi.me/partner/v1.0/ats/assessment_invitations/{invitation_code} or

provide a **webhook callback url** that accepts **HTTP post** method to Pulsifi

A **client ID** value will be included in the **HTTP post's header** with name of "client-id", which used to determine Pulsifi as the source.

For best security practice, always **whitelist Pulsifi’s API platform IP** by requesting IP address from Pulsifi.

<br />
<br />

## Pulsifi Integration API Reference

<br />

## 1. Publish Job Endpoint (Coming soon)

-   This endpoint will be used to create a job.
    <br /><br />

#### URL (HTTP POST)

-   https://api.pulsifi.me/partner/v1.0/ats/jobs

#### Content Type

-   Format: application/json

#### Payload

<br />

  <table border=1>
  <tr>
  <th>Name</th>
  <th>Type</th>
  <th>Required?</th>
  <th>Description</th>
  </tr>

  <tr>
  <td>title</td>
  <td>String</td>
  <td>Y</td>
  <td>maximum 255 characters</td>
  </tr>

  <tr>
  <td>required_skills</td>
  <td>String (array)</td>
  <td>Y</td>
  <td>maximum 10 items</td>
  </tr>
  
  <tr>
  <td>description</td>
  <td>String</td>
  <td>N</td>
  <td>maximum 500 characters</td>
  </tr>

  <tr>
  <td>ext_reference_id</td>
  <td>String</td>
  <td>N</td>
  <td>ATS platform job id.
  <ul>
  <li>maximum 50 characters</li>
  </ul>
  </td>
  </tr>

  </table><br /><br />

#### Response
<br />
  <table border=1>
  <tr>
  <th>Attribute</th>
  <th>Type</th>
  <th>Description</th>
  </tr>

  <tr>
  <td>id</td>
  <td>String (uuid)</td>
  <td>Job ID provided by Pulsifi.</td>
  </tr>

  <tr>
  <td>title</td>
  <td>String</td>
  <td></td>
  </tr>
  <tr>
  <td>required_skills</td>
  <td>String (array)</td>
  <td></td>
  </tr>

  <tr>
  <td>description</td>
  <td>String</td>
  <td></td>
  </tr>

  <tr>
  <td>status</td>
  <td>String</td>
  <td><strong>active</strong>, initial state</td>
  </tr>
  
  <tr>
  <td>assessments</td>
  <td>String (array)</td>
  <td>
    <strong>work_interest</strong>. <strong>personality</strong>, <strong>work_value</strong>, <strong>reasoning_verbal</strong>, <strong>reasoning_logic</strong>, <strong>reasoning_numeric</strong>
  </td>
  </tr>

  <tr>
  <td>created_at</td>
  <td>UTC Date</td>
  <td>
  Record created date.<br />
  Example: 2021-08-04T23:59:59.999Z
  </td>
  </tr>

  </table><br /><br />

#### Sample Publish Job Post Request

<br />

```
curl -X 'POST' \
'https://api.pulsifi.me/partner/v1.0/ats/jobs' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer <access_token>' \
  -H 'Content-Type: application/json' \
  -d '{
  "title": "Senior Data Scientist",
  "required_skills": ["Python", "Machine Learning", "...."],
  "description": "We are seeking a talented and motivated Data Scientist to join our team. As a Data Scientist, you will play a critical role in analyzing complex datasets...",
  "ext_reference_id": "ats-001"
}'
```

<br />  

## 2. Generate Pulsifi Assessment Invitation Endpoint

-   This endpoint will be used to generate a Pulsifi's assessment invitation link.
    <br /><br />

#### URL (HTTP POST)

-   https://api.pulsifi.me/partner/v1.0/ats/assessment_invitations

#### Content Type

-   Format: application/json

#### Payload

<br />

  <table border=1>
  <tr>
  <th>Name</th>
  <th>Type</th>
  <th>Required?</th>
  <th>Description</th>
  </tr>

  <tr>
  <td>job_id</td>
  <td>String (uuid)</td>
  <td>Y</td>
  <td>Refer to FAQ section below</td>
  </tr>

  <tr valign=top>
  <td>ext_reference_id</td>
  <td>String</td>
  <td>Y</td>
  <td>
  ATS platform job application id.
  <ul>
  <li>maximum 50 characters</li>
  </ul>
  </td>
  </tr>

  <tr valign=top>
  <td>email</td>
  <td>String</td>
  <td>Y
  </td>
  <td>
  Candidate's email.
  <ul>
  <li>maximum 255 characters</li>
  </ul>
  </td>
  </tr>

  <tr valign=top>
  <td>first_name</td>
  <td>String</td>
  <td>Y
  </td>
  <td>
  Candidate's first name.
  <ul>
  <li>maximum 255 characters</li>
  </ul>
  </td>
  </tr>

  <tr valign=top>
  <td>last_name</td>
  <td>String</td>
  <td>Y
  </td>
  <td>
  Candidate's last name.
  <ul>
  <li>maximum 255 characters</li>
  </ul>
  </td>
  </tr>

  <tr valign=top>
  <td>skills (Coming soon)</td>
  <td>String (array)</td>
  <td>
  N
  </td>
  <td>
  Candidate's skills
  <ul>
  <li>maximum 20 items</li>
  </ul>
  Example: <br/><i>"skills": [
  "Python","AWS","Leadership"
  ]
  </td>
  </tr>

  <tr valign=top>
  <td>work_experiences (Coming soon)</td>
  <td>Array of work_experience:{ <br/>
    role:string (Y)<br/> 
    organization (N)<br/>
    responsibility_achievement:string (N)<br/> 
    start_date:date (Y)<br/>
    end_date:date (N)<br/>
    }<br/>
  </td>
  <td>
  N
  </td>
  <td>
  Candidate's work exprience
  <ul>
  <li>maximum 10 items</li>
  </ul>
  Example: <br/><i>"work_experiences": [
  {
    "role": "Tech Lead",
    "organization": "Pulsifi",
    "responsibility_achievement": "Leading a team of 6",
    "start_date": "2023-02-15',
    "end_date": "",
  },<br/>
  {
    "role": "Senior Software Engineer",
    "organization": "Tesla",
    "responsibility_achievement": "Deliver feature on time with high code quality",
    "start_date": "2020-04-01',
    "end_date": "2023-01-30"
  }    
]</i><br/>
  </td>
  </tr>

  <tr valign=top>
  <td>deadline</td>
  <td>UTC Date</td>
  <td>N</td>
  <td>
  Pulsifi's invitation link deadline date (UTC date).<br />
  <ul>
  <li>Default to 3 months validity period</li>
  <li>maximum 3 months validity period</li>
  <li>Example: "2021-08-12T12:21:59Z"</li>
  </ul>  
  </td>
  </tr>

  </table><br /><br />

#### Response

<br />
  <table border=1>
  <tr>
  <th>Attribute</th>
  <th>Type</th>
  <th>Description</th>
  </tr>

  <tr>
  <td>status</td>
  <td>String</td>
  <td><strong>invited</strong>, initial state</td>
  </tr>

  <tr>
  <td>ext_reference_id</td>
  <td>String</td>
  <td>ATS platform job application id.</td>
  </tr>

  <tr>
  <td>job_id</td>
  <td>String (uuid)</td>
  <td>Job ID provided by Pulsifi.</td>
  </tr>

  <tr>
  <td>invitation_code.</td>
  <td>String</td>
  <td>Pulsifi's invitation code. <br/>
  <strong>Recommend to store this code in your database as reference <br>
  for further assessment status and result query</strong></td>
  </tr>

  <tr>
  <td>invitation_link</td>
  <td>String</td>
  <td>
  Pulsifi's assessment invitation link.<br />
  Example: https://candidate.pulsifi.me/invites/...
  </td>
  </tr>

  <tr>
  <td>invitation_expired_at</td>
  <td>UTC Date</td>
  <td>
  Pulsifi's assessment invitation link expiry date.<br />
  Example: 2021-08-04T23:59:59.999Z
  </td>
  </tr>

  <tr>
  <td>created_at</td>
  <td>UTC Date</td>
  <td>
  Pulsifi's assessment invitation link created on this date.<br />
  Example: 2021-08-04T23:59:59.999Z
  </td>
  </tr>

  </table>
<br /><br />

#### Sample Invitation Post Request

<br />

```
curl -X 'POST' \
'https://api.pulsifi.me/partner/v1.0/ats/assessment_invitations' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer <access_token>' \
  -H 'Content-Type: application/json' \
  -d '{
  "job_id": "<pulsifi job id>",
  "ext_reference_id": "<ATS reference id>",
  "email": "tester@test.com",
  "first_name": "Tester",
  "last_name": "User",
  "deadline": "2021-08-12T12:21:59Z"
}'
```

<br />

## 3. Get Assessment Invitation Status

-   This endpoint will be used to get assessment invitation status for progress tracking purpose.
    <br /><br />

#### URL (HTTP GET)

-   https://api.pulsifi.me/partner/v1.0/ats/assessment_invitations/{invitation_code}
-   Path Param
    -   **_invitation_code_**, getting from generate assessment invitation response

#### Content Type

-   Format: application/json

#### Response

<br />
  <table border=1>
  <tr>
  <th>Attribute</th>
  <th>Type</th>
  <th>Description</th>
  </tr>

  <tr>
  <td>status</td>
  <td>String</td>
  <td>
    <ul>
    <li><strong>invited</strong>, initial state</li>
    <li><strong>expired</strong>, assessment link expired</li>
    <li><strong>opened</strong>, after candidate accept the assessment link</li>
    <li><strong>started</strong>, after candidate complete at least 1 assessment</li>
    <li><strong>completed</strong>, after candidate complete all assessment</li>
  </ul>
    </td>
  </tr>

  <tr>
  <td>ext_reference_id</td>
  <td>String</td>
  <td>ATS platform job application id.</td>
  </tr>

  <tr>
  <td>job_id</td>
  <td>String (uuid)</td>
  <td>Job ID provided by Pulsifi.</td>
  </tr>

  <tr>
  <td>invitation_code.</td>
  <td>String</td>
  <td>Pulsifi's invitation code</td>
  </tr>

  <tr>
  <td>invitation_link</td>
  <td>String</td>
  <td>
  Pulsifi's assessment invitation link.<br />
  Example: https://candidate.pulsifi.me/invites/...
  </td>
  </tr>

  <tr>
  <td>invitation_expired_at</td>
  <td>UTC Date</td>
  <td>
  Pulsifi's assessment invitation link expiry date.<br />
  Example: 2021-08-04T23:59:59.999Z
  </td>
  </tr>

  <tr>
  <td>created_at</td>
  <td>UTC Date</td>
  <td>
  Pulsifi's assessment invitation link created on this date.<br />
  Example: 2021-08-04T23:59:59.999Z
  </td>
  </tr>

  </table>
<br /><br />

## 4. Get Assessment Result

-   There are 2 ways in retrieving candidate's assessment result after candidate completed all assessments.

### 4.1 Get Assessment Result via HTTP GET

#### URL (HTTP GET)

-   https://api.pulsifi.me/partner/v1.0/ats/assessment_invitations/{invitation_code}/result
-   Path Param
    -   **_invitation_code_**, getting from generate assessment invitation response

#### Content Type

-   Format: application/json

#### Response

<br />
  <table border=1>
  <tr>
  <th>Attribute</th>
  <th>Type</th>
  <th>Description</th>
  </tr>

  <tr>
  <td>invitation_code</td>
  <td>String</td>
  <td>Pulsifi's invitation code</td>
  </tr>

  <tr>
  <td>scores</td>
  <td>Array of score:{ <br/>
    score_type:string, <br/> 
    score_value:number,<br/> 
    score_format:number 
    }<br/>
  </td>
  <td>Pulsifi's fit score. <br/><strong>No of fit score items returned is determine by the Pulsifi Job setup.</strong><br/>
    Possible display's data: <br/>
  - <i>Pulsifi Role Fit Score</i><br/>
  - <i>Pulsifi Organizational Fit Score</i><br/><br/>
  score_value's data:<br/>
   - <i>minimum : 0</i><br/>
   - <i>maximum : 100</i><br/><br/>
   Example: <br/><i>"scores": [
    {
      "score_type": "role_fit",
      "score_value": 67,
      "score_format": 100,
    },<br/>
    {
      "score_type": "org_fit",
      "score_value": 45
      "score_format": 100
    }    
  ]</i><br/>
  </td>
  </tr>

  <tr>
  <td>report_profile_link</td>
  <td>String</td>
  <td>Pulsifi's assessment report with 1 year validity period.<br/>
    Example: <i>https://app.pulsifi.me/share/candidate/...</i>
  </td>
  </tr>

  <tr>
  <td>report_pdf_link  (Coming soon)</td>
  <td>String</td>
  <td>Pulsifi's assessment report report in pdf link with expiration up to hours.<br/>
    Example: <i>https://document.pulsifi.me/candidate/xxx/report.pdf</i>
  </td>
  </tr>


  </table>
<br /><br />

### 4.2 Get Assessment Result via Webhook

<br />

The ATS platform will be required to provide a **webhook callback url** <br/>
**HTTP POST** will be triggered when candidate assessment result is ready.
<br /><br />

### URL (HTTPS POST)

-   Webhook URL to be provided by the ATS platform
-   Sample Response: 200 OK

### Header

-   header 'client-id: sample-client-id'
-   <i>'sample-client-id'</i> to be provided by Pulsifi

### Payload

-   Format: application/json

<br />

  <table border=1>
  <tr>
  <th>Name</th>
  <th>Type</th>
  <th>Description</th>
  </tr>

  <tr valign=top>
  <td>event_type</td>
  <td>String</td>
  <td>Pulsifi's event type<br />
  Example: candidate_assessment__role_fit_score_ready / candidate_assessment_org_fit_score_ready
  </td>
  </tr>
  
  <tr>
  <td>job_id</td>
  <td>String (uuid)</td>
  <td>Job ID provided by Pulsifi.</td>
  </tr>

  <tr>
  <td>ext_reference_id</td>
  <td>String</td>
  <td>ATS platform job application id.</td>
  </tr>

  <tr>
  <td>report_profile_link</td>
  <td>String</td>
  <td>
  Pulsifi's assessment report link with 1 year validity period. <br />
  Example: https://app.pulsifi.me/share/candidate/...
  </td>
  </tr>

  <tr>
  <td>report_pdf_link (Coming soon)</td>
  <td>String</td>
  <td>
  Pulsifi's assessment report report in pdf link with expiration up to hours. <br />
    Example: https://document.pulsifi.me/candidate/xxx/report.pdf
  </td>
  </tr>

  <tr>
  <td>invitation_code</td>
  <td>String</td>
  <td>
  Pulsifi's invitation code
  </td>
  </tr>

  <tr>
  <td>fit_score</td>
  <td>Number</td>
  <td>
  Pulsifi's Fit Score (Role Fit Score/ Org Fit Score).
  </td>
  </tr>

  </table><br />

### Sample Pulsifi Role Fit Score Payload

<br />

```
{
  "event_type": "candidate_assessment_role_fit_score_ready",
  "event_body": {
    "job_id": "<pulsifi job id>",
    "ext_reference_id": "<ATS job application reference id>",
    "report_profile_link": "https://app.pulsifi.me/share/candidate/...",
    "report_pdf_link": "https://document.pulsifi.me/candidate/xxx/report.pdf",
    "invitation_code": "<pulsifi job application id reference>",
    "score_type":"role_fit",
    "score_value": 94,
    "score_format: 100,
  }
}
```

<br />

### Sample Pulsifi Org Fit Score Payload

<br />

```
{
  "event_type": "candidate_assessment_org_fit_score_ready",
  "event_body": {
    "job_id": "<pulsifi job id>",
    "ext_reference_id": "<ATS job application reference id>",
    "report_profile_link": "https://app.pulsifi.me/share/candidate/...",
    "report_link": "https://document.pulsifi.me/candidate/xxx/report.pdf",
    "invitation_code": "<pulsifi job application id reference>",
    "score_type":"org_fit",
    "score_value": 71,
    "score_format: 100,  
    }
}
```

## FAQ

### 1. How can i get Pulsifi Job Id

-   By default, you can always get it by accessing job module in the Pulsifi app.

    -   Select a job you want the candidate apply to and view the job detail
    -   The job id can be found on the browser url, example : *https://app.pulsifi.me/acquisition/jobs/{job_id}*

-   If you are the 3rd party ATS platform who did not have access to Pulsifi app, please request from Pulsifi account manager who manage the integration project.

-  If you want to start with Staging API Environment, please request from Pulsifi account manager who manage the integration project.

-  If you are partner, please use publish job endpoint to obtain a job id
