# Pulsifi Platform API Integration Guideline

## Authentication (Basic Auth)

<br />

Pulsifi's Integration Team will provide the necessary **API key/client ID**

Pulsifi integration API calls will be authenticated using **basic authentication (base64 format)**.

The following is a sample of Basic Authentication:

```
username: <pulsifi_api_key>
password: <blank, no password required>
```

The following is a sample of a Post request to Pulsifi’s platform API endpoints:

```
curl --request POST \
  --url 'https://api.pulsifi.me/public/invitations/ats' \
  --header 'Authorization: Basic YOUR-ENCODED-APIKEY'
  ...
```

## Capture and store assessment result

if assessment result data need to capture and store in your ATS platform, please provide a **webhook callback url** that accepts **HTTP post** method to Pulsifi

A **client ID** value will be included in the **HTTP post's header** with name of "client-id", which used to determine Pulsifi as the source.

For best security practice, always **whitelist Pulsifi’s platform IP** by requesting IP address from Pulsifi

<br />
<br />

## Pulsifi Integration API Reference

<br />

### 1. Generate Pulsifi Assessment Invitation Endpoint

-   This endpoint will be used to generate a Pulsifi's assessment invitation link.
    <br /><br />

#### URL (HTTP POST)

-   https://api.pulsifi.me/public/invitations/ats

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
  <td>is_anonymous_candidate</td>
  <td>Boolean</td>
  <td>Y</td>
  <td>
  Options:
  <ul>
  <li>true: Anonymous mode</li>
  <li>false: Non-anonymous mode</li>
  </ul>
  </td>
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
  <td>
  Y (non-anonymous mode)<br />
  N (anonymous mode)
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
  <td>
  Y (non-anonymous mode)<br />
  N (anonymous mode)
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
  <td>
  Y (non-anonymous mode)<br />
  N (anonymous mode)
  </td>
  <td>
  Candidate's last name.
  <ul>
  <li>maximum 255 characters</li>
  </ul>
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

  <tr valign=top>
  <td>is_anonymous_candidate</td>
  <td>Boolean</td>
  <td>
  Options:
  <ul>
  <li>true: Anonymous mode</li>
  <li>false: Non-anonymous mode</li>
  </ul>
  </td>
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

#### Sample Invitation Post Request (anonymous)

<br />

```
curl -X 'POST' \
'https://api.pulsifi.me/public/invitations/ats' \
  -H 'accept: application/json' \
  -H 'Authorization: Basic YWRtaW46YWFhYQ==' \
  -H 'Content-Type: application/json' \
  -d '{
  "job_id": "<pulsifi job id>",
  "ext_reference_id": "<ATS reference id>",
  "is_anonymous_candidate": true,
  "deadline": "2021-08-12T12:21:59Z"
}'

```

<br />

#### Sample Invitation Post Request (non-anonymous)

<br />

```
curl -X 'POST' \
'https://api.pulsifi.me/public/invitations/ats' \
  -H 'accept: application/json' \
  -H 'Authorization: Basic YWRtaW46YWFhYQ==' \
  -H 'Content-Type: application/json' \
  -d '{
  "job_id": "<pulsifi job id>",
  "ext_reference_id": "<ATS reference id>",
  "is_anonymous_candidate": false,
  "email": "tester@test.com",
  "first_name": "Tester",
  "last_name": "User",
  "deadline": "2021-08-12T12:21:59Z"
}'
```

<br />

### 2. Get Assessment Invitation Status

-   This endpoint will be used to get assessment invitation status for progress tracking purpose.
    <br /><br />

#### URL (HTTP GET)

-   https://api.pulsifi.me/public/invitations/ats/{invitation_code}
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

  <tr valign=top>
  <td>is_anonymous_candidate</td>
  <td>Boolean</td>
  <td>
  Options:
  <ul>
  <li>true: Anonymous mode</li>
  <li>false: Non-anonymous mode</li>
  </ul>
  </td>
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

### 3. Get Assessment Invitation Result

-   This endpoint will be used to get assessment invitation result after candidate complete all assessment(s).
    <br /><br />

#### URL (HTTP GET)

-   https://api.pulsifi.me/public/invitations/ats/{invitation_code}/result
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
  <td>Array of score:{ display:string, value:number }</td>
  <td>Pulsifi's fit score<br/>
   Example: <i>"scores": [
    {
      "display": "Pulsifi Role Fit Score",
      "value": "45"
    }
  ]</i>
  </td>
  </tr>

  <tr>
  <td>report_url</td>
  <td>String</td>
  <td>Pulsifi's assessment report url<br/>
    Example: <i>https://app.pulsifi.me/share/candidate/...</i>
</td>
  </tr>

  </table>
<br /><br />

### 3.1 Get Assessment Invitation Result via Webhook

<br />

The ATS platform will be required to provide a **webhook callback url** <br/>
**HTTP POST** will be fired when candidate fit score is ready.
<br /><br />

### URL (HTTPS POST)

-   Webhook URL to be provided by the ATS platform
-   Sample Response: 200 OK

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
  Example: job_application_role_fit_score_created / job_application_culture_fit_score_created
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
  <td>profile_share_url</td>
  <td>String</td>
  <td>
  Pulsifi's public profile share link.<br />
  Example: https://app.pulsifi.me/share/candidate/...
  </td>
  </tr>

  <tr>
  <td>job_application_id</td>
  <td>String (uuid)</td>
  <td>
  Pulsifi's job application ID reference.
  </td>
  </tr>

  <tr>
  <td>fit_score</td>
  <td>Number</td>
  <td>
  Pulsifi's Fit Score (Role Fit Score/ Culture Fit Score).
  </td>
  </tr>

  </table><br />

### Sample Pulsifi Role Fit Score Payload

<br />

```
{
  "event_type": "job_application_role_fit_score_created",
  "event_body": {
    "job_id": "<pulsifi job id>",
    "ext_reference_id": "<ATS job application reference id>",
    "profile_share_url": "https://app.pulsifi.me/share/candidate/...",
    "job_application_id": "<pulsifi job application id reference>",
    "fit_score": 9.4
  }
}
```

<br />

### Sample Pulsifi Culture Fit Score Payload

<br />

```
{
  "event_type": "job_application_culture_fit_score_created",
  "event_body": {
    "job_id": "<pulsifi job id>",
    "ext_reference_id": "<ATS job application reference id>",
    "profile_share_url": "https://app.pulsifi.me/share/candidate/...",
    "job_application_id": "<pulsifi job application id reference>",
    "fit_score": 9.4
  }
}
```

## FAQ

### 1. How can i get Pulsifi Job Id

-   By default, you can always get it by accessing job module in the Pulsifi app.

    -   Select a job you want the candidate apply to and view the job detail
    -   The job id can be found on the browser url, example : *https://app.pulsifi.me/acquisition/jobs/{job_id}*

-   If you are the 3rd party ATS platform who did not have access to Pulsifi app, please request from Pulsifi account manager who manage the integration project.
