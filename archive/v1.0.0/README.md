# Pulsifi Platform API Integration Guideline v1.0.0

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
  --url 'https://api.pulsifi.me/partner/oauth2/token' \
  --header 'Authorization: Basic YOUR-ENCODED-CLIENT-ID-CLIENT-SECRET' \
  ...
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

## 1. Generate Pulsifi Assessment Invitation Endpoint

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
'https://api.pulsifi.me/partner/v1.0/ats/assessment_invitations' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer <access_token>' \
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
'https://api.pulsifi.me/partner/v1.0/ats/assessment_invitations' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer <access_token>' \
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

## 2. Get Assessment Invitation Status

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

## 3. Get Assessment Result

-   There are 2 ways in retrieving candidate's assessment result after candidate completed all assessments.

### 3.1 Get Assessment Result via HTTP GET

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
  <td>Array of score:{ display:string, value:number }<br/>
  </td>
  <td>Pulsifi's fit score. <br/><strong>No of fit score items returned is determine by the Pulsifi Job setup.</strong><br/>
    Possible display's data: <br/>
  - <i>Pulsifi Role Fit Score</i><br/>
  - <i>Pulsifi Organizational Fit Score</i><br/><br/>
  value's data:<br/>
   - <i>minimum : 0</i><br/>
   - <i>maximum : 100</i><br/><br/>
   Example: <br/><i>"scores": [
    {
      "display": "Pulsifi Role Fit Score",
      "value": "67"
    },<br/>
    {
      "display": "Pulsifi Organizational Fit Score",
      "value": "45"
    }    
  ]</i><br/>
  </td>
  </tr>

  <tr>
  <td>report_url</td>
  <td>String</td>
  <td>Pulsifi's assessment report url with 1 year validity period.<br/>
    Example: <i>https://app.pulsifi.me/share/candidate/...</i>
</td>
  </tr>

  </table>
<br /><br />

### 3.2 Get Assessment Result via Webhook

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
  Pulsifi's assessment report url with 1 year validity period. <br />
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
