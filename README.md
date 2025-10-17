# API authentication and consumption

# Consuming CrowdStrike CSPM Oauth2 based API

# Important: The CrowdStrike API uses OAuth2 for authentication.

- OAuth2 enables you to do the following:
- Use access tokens to make API requests
- Manage multiple API clients within your organization
- Define limited scopes of permissions for API functionality

- Below is classic OAuth2-style workflow used by many enterprise APIs, including CrowdStrike’s.
# Important: 
- Each API endpoint will require an API key you use to get an access token during a post request. These API keys will require different scope of permissions like read/write.

  
- 1. 🔑 Creating the API Key / Client Credentials in Falcon CSPM 
  - Note: Ensure the API key has right scope of permissions for certain API endpoints
  - 1. Create the API key in Falcon tool and ensure defined scopes/permissions are met based on CSPM API endpoint.
  - - You can find required permissions in the API documentation
  - API key will have client ID + client secret. Store in a secure place like Key Vault.
  - API key act as credentials for your request to the API endpoint (sort of like a username/password pair for authentication).
  - Read Only permissions have been assigned to this specific API key and it's required to make GET requests to the URL: "https://api.crowdstrike.com/settings/entities/policy/v1?service=" to retrieve CrowdStrike security policies by service
  - <img width="520" height="103" alt="image" src="https://github.com/user-attachments/assets/79a2c4c0-f7e1-414f-860a-001da688ccea" />

 
- 🧾 2. Requesting an Access Token (Authentication)
  - Before you make the POST to URL https://api.crowdstrike.com/oauth2/token , ensure headers and body are correct
  - Headers should contain Content-Type: application/x-www-form-urlencoded
  - <img width="1140" height="293" alt="image" src="https://github.com/user-attachments/assets/8408cd08-18a9-4c5d-9983-2070d311f05f" />

  - Body should contain client_id=<your_client_id> & client_secret=<your_client_secret> and grant_type = client_credentials
<img width="1279" height="817" alt="image" src="https://github.com/user-attachments/assets/aeeb6b2b-e8be-4757-9088-1175c2944e88" />

  - As shown above screenshot, when you make the POST API request with your API key to url https://api.crowdstrike.com/oauth2/token, you will retrieve an access token aka bearer token

  Note: The API returns a Bearer token (access token) — typically valid for 30 minutes or so.

 
- ⚙️ 3. Making Authenticated API Requests
  - Once you have the bearer token (access token), you include it in the Authorization header of your next requests:
  - That is how you authenticate and prove who you say you are to the API endpoint.
  - GET "https://api.crowdstrike.com/settings/entities/policy/v1?service=Detective"
  - Authorization: Bearer <access_token>
  - I am making a GET API request to the CSPM API to retrieve a list of all policies pertaining to the service "Detective"
    -   <img width="1302" height="582" alt="image" src="https://github.com/user-attachments/assets/e0120bcc-af10-4334-96d3-e2d605979a88" />
    - If I want to get another list of policies for a different service, I modify the url to be "S3"
    - Example: "https://api.crowdstrike.com/settings/entities/policy/v1?service=S3"
    - <img width="1279" height="839" alt="image" src="https://github.com/user-attachments/assets/808afcfd-56d4-4847-aebb-12677770d767" />


- 4. Save the file and clean it with Python
     <img width="1275" height="683" alt="image" src="https://github.com/user-attachments/assets/79dd06bb-5b69-489b-b8dc-7254c247d2d0" />


     Python code that will take ALL the json files in that directory/folder (aws_json_security_policies) and combine them into an excel file called aws_combined_resources.xlsx
     
import json
import pandas as pd
import glob

# List all JSON files in a folder (change path as needed)
json_files = glob.glob('/Users/n664768/PycharmProjects/PythonProject/aws_json_security_policies/*.json')

# List to hold all extracted data from multiple files
all_extracted_data = []

# Process each JSON file
for file_path in json_files:
    with open(file_path, 'r') as file:
        data = json.load(file)

    # Assuming 'resources' is the key containing the list in each JSON file
    if 'resources' in data:
        resources = data['resources']
        for resource in resources:
            if isinstance(resource, dict):  # Ensure resource is a dictionary
                all_extracted_data.append({
                 "cloud_service_friendly": resource.get("cloud_service_friendly"),
                 "policy_id": resource.get("policy_id"),
                 "name": resource.get("name"),
                 "default_severity": resource.get("default_severity"),
                 "policy_type": resource.get("policy_type"),
                })

# Create a DataFrame from all the extracted data
df = pd.DataFrame(all_extracted_data)

# Write the DataFrame to an Excel file
df.to_excel('aws_combined_resources.xlsx', index=False, engine='openpyxl')

print("Data from all files successfully written to 'aws_combined_resources.xlsx'.")





