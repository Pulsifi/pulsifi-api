# pulsifi-api
Pulsifi Platform API Integration Guideline

### Prerequisite 
1.You should have in-depth knowledge about 0Auth2.0 Authorization Flow
2. If you wish to be Pulsifi Platform Partner, contact Pulsifi Integration Team for information as below
    - **Client Credential**
        - Please provide a **Callback Url** for whitelist purpose,  you will need this under Authentication section
3. If you wish to managing data via API on behalf of Pulsifi client, contact Pulsifi Integration Team for information as below
    - **API User Credential** : we recommend username start with api_xxx@domain, for example api_user@pulsifi.me together with the actual password
    - **Company Id** : Must be a valid company id that accessible by Api User

### Authentication
1. In order to obtain a Acess Token, you need to follow 0Auth2.0 Authorization Flow 
    - Authorize URL: https://sandbox-id.pulsifi.me/authorize. **Callback Url** is required in order to receive the **Authorize Code**
    - Token URL: https://sandbox-id.pulsifi.me/oauth/token. Use **Authorize Code** here to obtain access token

### API Method

#### **(TBA)** Create Job 
1. create job opening with job title 
#### **(TBA)** Generate Invite Link 
1. create invitation link to take assessment 

###  Webhook Callback 

-1. **(TBA)** Send Assessment Score, Profile Link when Candidate Completed Assessment 
