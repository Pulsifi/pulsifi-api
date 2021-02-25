# pulsifi-api
(WIP) Pulsifi Platform API Integration Guideline 

### Prerequisite 

1. Obtain api-key and client-id from Pulsifi integration team
    - Provide the target company id
2. Provide your **Webhook callback url** that accept HTTP POST method.
    - **client-id** value will be included in the header of "client-id". Please compare this with the client-id value issued in step.1

### Authentication
1. In order to use Pulsifi integration api, please add "api-key" header in every http api call.


### API Method
#### Generate Invite Link 
1. create invitation link to take assessment 

  ### URL (HTTP POST)
  - https://api.pulsifi.me/integration/application_invite

  ### Payload

  - job_id *
  - email *
  - first_name *
  - last_name *


  ### Response
  
  - job_id
  - email
  - invitation_code
  - invitation_link
  

###  Webhook Callback 

1. Push Fit Score, Candidate Link when Candidate Completed Assessment

  ### Response
  - candidate_id 
  - job_id
  - first_name
  - last_name
  - email
  - role_fit_score
  - candidate_link: https://app.pulsifi.me/candidates/{id}

