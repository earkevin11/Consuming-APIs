# API authentication and consumption

# Consuming CrowdStrike CSPM API

- Below is classic OAuth2-style workflow used by many enterprise APIs, including CrowdStrike’s.
- Important: Each API endpoint may require an API key you use to get an access token during a post request to have different scope of permissions like read/write.
- URL for retrieving an access key with your API client secret and client ID
- POST with your API key to url https://api.crowdstrike.com/oauth2/token

  
- 1. 🔑 Creating the API Key / Client Credentials in Falcon CSPM a
  Note: Ensure the API key has the right scope of permissions for certain API endpoints
  - You will then get a client ID + client secret
  - These act as credentials for your application (sort of like a username/password pair for automation).
  - Confirm you assigned it the right scopes or permissions — that’s key because CrowdStrike’s API is granular (e.g. read-only for detections, full access for configurations, etc.).
 
- 🧾 2. Requesting an Access Token (Authentication)
  - You then take the client secret + client ID and put them in a API client like Postman and then make a POST request to get an access token.
  - POST https://api.crowdstrike.com/oauth2/token
  - Content-Type: application/x-www-form-urlencoded
  - client_id=<your_client_id>&client_secret=<your_client_secret>
  Note: The API returns a Bearer token (access token) — typically valid for 30 minutes or so.

 
- ⚙️ 3. Making Authenticated API Requests
  - Once you had the bearer token (access token), you includ it in the Authorization header of your next requests:
  - You then take the access token and put in your request to GET,POST, or PATCH. That is how you authenticate and prove who you say you are.
  - GET https://api.crowdstrike.com/devices/queries/devices/v1
  - Authorization: Bearer <access_token>


  - OR
  - POST https://api.crowdstrike.com/devices/entities/devices-actions/v2
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "action_name": "contain",
  "ids": ["<device_id>"]
}


