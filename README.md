# Pulsifi Platform API Integration Guideline

### Prerequisite

1. Obtain **api-key, client-id** and necessary **Pulsifi job ID** from Pulsifi integration team
2. Provide your **Webhook callback url** that accept HTTP POST method.
   - **client-id** value will be included in the header of "client-id". Please compare this with the client-id value issued in step.1
     <br />

### Authentication

1. In order to use Pulsifi integration api, please add the "api-key" header to every http api call.
   <br /><br />

### API Method

#### Generate Invite Link

1. Create Pulsifi's invitation link to take assessment

#### URL (HTTP POST)

- http://api.pulsifi.me/integration/v1.0/invitation/ats

#### Payload

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

### ATS PLATFORM WEBHOOK INTEGRATION

1. Push Pulsifi's fit score, culture score, and public profile share link when candidate completes Pulsifi's assessment

#### URL (HTTP POST)

- Webhook URL to be provided by the ATS platform

#### Payload

- Format: application/json

<br />

  <table border=1>
  <tr>
  <th>Name</th>
  <th>Type</th>
  <th>Description</th>
  </tr>

  <tr>
  <td>event_type</td>
  <td>String</td>
  <td>Pulsifi's event type<br />
  Example: application_fit_score_created / application_culture_score_created
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
  <td>profile_link</td>
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
  Pulsifi's Fit Score.
  </td>
  </tr>

  <tr>
  <td>culture_score</td>
  <td>Number)</td>
  <td>
  Pulsifi's Culture Score.
  </td>
  </tr>

  </table><br />

#### Sample Pulsifi Fit Score Payload

<br />

```
{
  "event_type": "application_fit_score_created",
  "event_body": {
    "job_id": "<pulsifi job id>",
    "ext_reference_id": "<ATS job application reference id>",
    "profile_link": "https://app.pulsifi.me/share/candidate/...",
    "job_application_id": "<pulsifi job application id reference>",
    "fit_score": 9.4
  }
}
```

<br />

#### Sample Pulsifi Culture Score Payload

<br />

```
{
  "event_type": "application_culture_score_created",
  "event_body": {
    "job_id": "<pulsifi job id>",
    "ext_reference_id": "<ATS job application reference id>",
    "profile_link": "https://app.pulsifi.me/share/candidate/...",
    "job_application_id": "<pulsifi job application id reference>",
    "culture_score": 9.4
  }
}
```
