# Pulsifi Platform API Integration Guideline

## INTEGRATION REQUESTS AND TOKENS
<br />

Pulsifi's Integration Team will provide th necessary **API key, client ID** and the necessary **Pulsifi job ID**.

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
  --header 'Authorization: Basic YOURENCODEDAPIKEY'
  ...
```

The ATS platform will need to provide a **webhook callback url** that accepts **HTTP post** method, if data is required to be transferred back from Pulsifi's platform. The **client ID** value will be included in the header of "client-id", which will be used to determine Pulsifi as the source.

For security purposes, the ATS platform will be required to **whitelist Pulsifi’s platform IP** if data transfer back to the ATS platform is required.
<br />

   <br />

## PULSIFI'S API ENDPOINTS INTEGRATION
<br />

### Generate Pulsifi Invitation Link Endpoint

- This endpoint will be used to generate a Pulsifi's invitation link to take assessment.
<br /><br />

### URL (HTTP POST)

- https://api.pulsifi.me/public/invitations/ats

### Payload

- Format: application/json

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
  <td>Job ID provided by Pulsifi.</td>
  </tr>

  <tr valign=top>
  <td>is_anonymous_candidate</td>
  <td>Boolean</td>
  <td>Y</td>
  <td>
  Options:
  <ul>
  <li>true: Anonymous for non-PII mode</li>
  <li>false: Non anonymous for PII mode</li>
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
  <li>minimum 0 characters</li>
  <li>maximum 50 characters</li>
  </ul>
  </td>
  </tr>

  <tr valign=top>
  <td>email</td>
  <td>String</td>
  <td>
  Y (PII mode)<br />
  N (non-PII mode)
  </td>
  <td>
  Candidate's email.
  <ul>
  <li>minimum 0 characters</li>
  <li>maximum 255 characters</li>
  </ul>
  </td>
  </tr>

  <tr valign=top>
  <td>first_name</td>
  <td>String</td>
  <td>
  Y (PII mode)<br />
  N (non-PII mode)
  </td>
  <td>
  Candidate's first name.
  <ul>
  <li>minimum 0 characters</li>
  <li>maximum 255 characters</li>
  </ul>
  </td>
  </tr>

  <tr valign=top>
  <td>last_name</td>
  <td>String</td>
  <td>
  Y (PII mode)<br />
  N (non-PII mode)
  </td>
  <td>
  Candidate's last name.
  <ul>
  <li>minimum 0 characters</li>
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
  Example: "2021-08-12T12:21:59Z"
  </td>
  </tr>

  </table><br /><br />

### Response

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
  <td>SUCCESS (200) / ERROR (4XX / 5XX), status of the request.</td>
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
  <li>true: Anonymous for non-PII mode</li>
  <li>false: Non anonymous for PII mode</li>
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

### Sample Invitation Post Request (anonymous)

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

### Sample Invitation Post Request (non-anonymous)

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

## ATS PLATFORM WEBHOOK INTEGRATION

<br />

The ATS platform will be required to provide a **webhook callback url** if it requires Pulsifi’s platform to return Pulsifi's fit score, culture score, and public profile share link when candidate completes Pulsifi's assessment.
<br /><br />

### URL (HTTPS POST)

- Webhook URL to be provided by the ATS platform
- Sample Response: 200 OK


### Payload

- Format: application/json

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
