# Consuming-APIs

- Consuming CrowdStrike CSPM API
- 1. You may need to create an API key in Falcon CSPM and ensure the API key you are creating has the right scope of permissions for certain API endpoints
  - You will then get a client ID + client secret
  - These act as credentials for your application (sort of like a username/password pair for automation).
  - You assigned it the right scopes or permissions — that’s key because CrowdStrike’s API is granular (e.g. read-only for detections, full access for configurations, etc.).

  3. You then take the client secret + client ID and put in a API client like Postman and then make a POST request to get an access token.
  - POST https://api.crowdstrike.com/oauth2/token
  - Content-Type: application/x-www-form-urlencoded
  - client_id=<your_client_id>&client_secret=<your_client_secret>
  Note: The API returns a Bearer token (access token) — typically valid for 30 minutes or so.

 
  5. You then take the access token and put in your request to GET,POST, or PATCH. That is how you authenticate and prove who you say you are.
