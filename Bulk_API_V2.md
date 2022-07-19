## Bulk API V2 example using Postman

#### Step 1: Get Access Token(Username Password Flow)
 
  Endpoint: {{site_url}}/services/oauth2/token
  
  HTTP Method: POST
  
  Params:
    client_id - 3MVG9HxRZv05HarSrkgyjTBXAZhQ_Yln.U.ieXxGCt7yoOOosmtjyUZoQv6w87VE.sHuRaPSBUGxysZXGLfnS
    client_secret - 080A1024CEF305BB762643D5C0A3B8CBA64463318A5D28CBE595EB8E6D45343F
    grant_type - password
    username - rlakshman@gmail.com
    password - ******** + security token
    
  Sample Response:
    {
      "access_token": "00D0Y000000ocP4!ARgAQJS9NgAGjH_uVmqLoGi3yFRT9hvwM_TwL1vIVFkohwHFfQ8Dj5kFOAISjaJ0Ul62KxYaWtzP4WUV43n3W5zc7IIAEXSa",
      "instance_url": "https://rlakshman-dev-ed.my.salesforce.com",
      "id": "https://login.salesforce.com/id/00D0Y000000ocP4UAI/0050Y000000i6KTQAY",
      "token_type": "Bearer",
      "issued_at": "1658227065640",
      "signature": "rq3TiXxx2SdwWw4rbvkp8aGQRLyvOP0zIy09PLBTGhI="
    }
    
  
#### Step 2: Create Bulk Job

  Endpoint: {{site_url}}/services/data/v50.0/jobs/ingest/

  HTTP Method: POST

  Headers:
    Authorization: Bearer 00D0Y000000ocP4!ARgAQJS9NgAGjH_uVmqLoGi3yFRT9hvwM_TwL1vIVFkohwHFfQ8Dj5kFOAISjaJ0Ul62KxYaWtzP4WUV43n3W5zc6IIAEXSa
    Content-Type: application/json; charset=UTF-8
    Accept: application/json

  Body:
    {
      "object" : "Account",
      "contentType" : "CSV",
      "operation" : "insert",
      "lineEnding" : "CRLF"
    }

  Sample Response:
    {
      "id": "7501v00000u4PdKAAU",
      "operation": "insert",
      "object": "Account",
      "createdById": "0050Y000000i6KTQAY",
      "createdDate": "2022-07-19T10:58:28.000+0000",
      "systemModstamp": "2022-07-19T10:58:28.000+0000",
      "state": "Open",
      "concurrencyMode": "Parallel",
      "contentType": "CSV",
      "apiVersion": 50.0,
      "contentUrl": "services/data/v50.0/jobs/ingest/7501v00000u4PdKAAU/batches",
      "lineEnding": "CRLF",
      "columnDelimiter": "COMMA"
    }

#### Step 3: Add data to the Job

  Endpoint: {{site_url}}/services/data/v50.0/jobs/ingest/7501v00000u4PdKAAU/batches/

  HTTP Method: PUT

  Headers:
    Authorization: Bearer 00D0Y000000ocP4!ARgAQJS9NgAGjH_uVmqLoGi3yFRT9hvwM_TwL1vIVFkohwHFfQ8Dj5kFOAISjaJ0Ul62KxYaWtzP4WUV43n3W5zc6IIAEXSa
    Content-Type: text/csv
    Accept: text/csv

  Body(raw):
    Name,Email
    Test, test@test.com

  Sample Response:
    201 response

#### Step 4: Close the Job
  Endpoint: {{site_url}}/services/data/v50.0/jobs/ingest/7501v00000u4PdKAAU/batches/

  HTTP Method: PATCH

  Headers:
    Authorization: Bearer 00D0Y000000ocP4!ARgAQJS9NgAGjH_uVmqLoGi3yFRT9hvwM_TwL1vIVFkohwHFfQ8Dj5kFOAISjaJ0Ul62KxYaWtzP4WUV43n3W5zc6IIAEXSa
    Content-Type: application/json; charset=UTF-8
    Accept: application/json

  Body(raw):
    Name,Email
    Test, test@test.com

  Sample Response:
    {
      "state" : "UploadComplete"
    }

#### Step 5: Check Status of the Job
  Endpoint: {{site_url}}/services/data/v50.0/jobs/ingest/7501v00000u4PdKAAU/

  HTTP Method: GET

  Headers:
    Authorization: Bearer 00D0Y000000ocP4!ARgAQJS9NgAGjH_uVmqLoGi3yFRT9hvwM_TwL1vIVFkohwHFfQ8Dj5kFOAISjaJ0Ul62KxYaWtzP4WUV43n3W5zc6IIAEXSa
    Content-Type: application/json; charset=UTF-8
    Accept: application/json
  
  Sample Response:
    {
      "id": "7501v00000u4PdKAAU",
      "operation": "insert",
      "object": "Account",
      "createdById": "0050Y000000i6KTQAY",
      "createdDate": "2022-07-19T10:58:28.000+0000",
      "systemModstamp": "2022-07-19T11:09:44.000+0000",
      "state": "Failed",
      "concurrencyMode": "Parallel",
      "contentType": "CSV",
      "apiVersion": 50.0,
      "jobType": "V2Ingest",
      "lineEnding": "CRLF",
      "columnDelimiter": "COMMA",
      "numberRecordsProcessed": 0,
      "numberRecordsFailed": 0,
      "retries": 0,
      "totalProcessingTime": 0,
      "apiActiveProcessingTime": 0,
      "apexProcessingTime": 0,
      "errorMessage": "InvalidBatch : InvalidBatch : Field name not found : Email"
    }