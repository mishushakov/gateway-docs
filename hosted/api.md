# Dialogflow Gateway by Ushakov (Hosted) API

## Contents

- [API](#api)
  - [Response Codes](#response-codes)
  - [Errors](#errors)
  - [Endpoints](#endpoints)
    - [Base Endpoint](#base-endpoint)
    - [Base Endpoint Variables](#base-endpoint-variables)
    - [Base Endpoint Example](#base-endpoint-example)
- [Requests](#requests)
  - [Retrieving Agents](#retrieving-agents)
    - [Request](#request)
    - [Request Variables](#request-variables)
    - [Response Body](#response-body)
    - [Example Request](#example-request)
    - [Example Response](#example-response)
  - [Detecting Intents](#detecting-intents)
    - [Request](#request-1)
    - [Request Variables](#request-variables-1)
    - [Request Body](#request-body)
    - [Response Body](#response-body-1)
    - [Example Request](#example-request-1)
    - [Example Response](#example-response-1)
- [Realtime API](#realtime-api)
  - [Close Codes](#close-codes)
  - [Errors](#errors-1)
  - [Endpoints](#endpoints)
    - [Base Endpoint](#base-endpoint-1)
    - [Base Endpoint Variables](#base-endpoint-variables-1)
    - [Base Endpoint Example](#base-endpoint-example-1)
  - [Requests](#requests-1)
  - [Detecting Intents](#detecting-intents-1)
    - [Request](#request-2)
    - [Request Variables](#request-variables-2)
    - [Request Body](#request-body-1)
    - [Response Body](#response-body-2)
    - [Example Request](#example-request-2)
    - [Example Response](#example-response-2)
- [Contact](#contact)

## API

### Response Codes

| HTTP-Code | Reason                                                                         |
|-----------|--------------------------------------------------------------------------------|
| 200       | Request was successful                                                         |
| 400       | The request body or session ID is invalid and/or missing                       |
| 403       | The deployment was blocked or the service account key is no longer valid       |
| 404       | The deployment was not found                                                   |
| 500       | Internal server error                                                          |
| 503       | The service is unavailable                                                     |

### Errors

Example JSON response, containing error

```json
{"error": "Deployment was not found", "code": 404}
```

### Endpoints

#### Base Endpoint

```
https://<PROJECT_ID>.gateway.dialogflow.cloud.ushakov.co
```

#### Base Endpoint Variables

| Variable   | Description                  |
|------------|------------------------------|
| PROJECT_ID | Required, Project Identifier |

#### Base Endpoint Example

```
https://dialogflow-web-v2.gateway.dialogflow.cloud.ushakov.co
```

## Requests

#### Retrieving Agents

#### Request

```http
GET <BASE_ENDPOINT>
```

#### Request Variables

| Variable      | Description                 |
|---------------|-----------------------------|
| BASE_ENDPOINT | Required, Endpoint of Agent |

#### Response Body

[Agent](https://cloud.google.com/dialogflow/docs/reference/rest/v2beta1/projects.agent)

#### Example Request

```http
GET https://dialogflow-web-v2.gateway.dialogflow.cloud.ushakov.co
```

#### Example Response

```json
{
  "parent": "projects/dialogflow-web-v2",
  "displayName": "DialogflowWebV2",
  "defaultLanguageCode": "en",
  "supportedLanguageCodes": [
    "ru"
  ],
  "timeZone": "Europe/Madrid",
  "description": "This is a unofficial Progressive Web Application for Dialogflow V2, with support for rich-responses and amazing features you need to check out. Choose your language and send Hello to get started",
  "avatarUri": "https://storage.googleapis.com/cloudprod-apiai/ce408f19-7966-487d-8614-f5b1f0474ba6_x.png",
  "enableLogging": true,
  "matchMode": "MATCH_MODE_HYBRID",
  "classificationThreshold": 0.3
}
```

### Detecting Intents

#### Request

```http
POST <BASE_ENDPOINT>
```

#### Request Variables

| Variable | Description |
|----------|-------------|
| BASE_ENDPOINT | Required, Endpoint of Agent |

#### Request Body

[DetectIntentRequest](https://cloud.google.com/dialogflow/docs/reference/rpc/google.cloud.dialogflow.v2#google.cloud.dialogflow.v2.DetectIntentRequest)

#### Response Body

[DetectIntentResponse](https://cloud.google.com/dialogflow/docs/reference/rest/v2beta1/DetectIntentResponse)

#### Example Request

```http
POST https://app-of-the-day-9a9f6.gateway.dialogflow.cloud.ushakov.co
Content-Type: application/json

{
  "session": "test",
  "queryInput": {
    "text": {
      "text": "Hello",
      "languageCode": "en"
    }
  }
}
```

Note: Dialogflow Gateway converts `session` into `projects/<Project ID>/agent/sessions/<Session ID>`. This behaviour is a subject to change

#### Example Response

```json
{
  "responseId": "85c3d6bd-6f8c-4000-a1ec-fdd6f636b2e8",
  "queryResult": {
    "queryText": "Hello",
    "action": "input.welcome",
    "parameters": {},
    "allRequiredParamsPresent": true,
    "fulfillmentText": "Cannot display response in Dialogflow simulator. Please test on the Google Assistant simulator instead.",
    "fulfillmentMessages": [...],
    "webhookPayload": {...},
    "intent": {
      "name": "projects/app-of-the-day-9a9f6/agent/intents/1e00bd68-62f0-4b8e-9cdd-a81c19f73855",
      "displayName": "Default Welcome Intent"
    },
    "intentDetectionConfidence": 1,
    "diagnosticInfo": {
      "end_conversation": true,
      "webhook_latency_ms": 187
    },
    "languageCode": "en"
  },
  "webhookStatus": {
    "message": "Webhook execution successful"
  }
}
```

## Realtime API

Realtime API implements [Secure WebSocket](https://en.wikipedia.org/wiki/WebSocket) communication between Dialogflow Gateway and the clients

### Close Codes

| Code | Reason                                                                          |
|-----------|----------------------------------------------------------------------------|
| 4400      | The request body or session ID is invalid and/or missing                   |
| 4403      | The deployment was blocked or the service account key is no longer valid   |
| 4404      | The deployment was not found                                               |
| 4500      | Internal server error                                                      |

### Errors

Example error:

```
4404 'Deployment was not found'
```

### Endpoints

#### Base Endpoint

```
wss://<PROJECT_ID>.gateway.dialogflow.cloud.ushakov.co
```

#### Base Endpoint Variables

| Variable   | Description                  |
|------------|------------------------------|
| PROJECT_ID | Required, Project Identifier |

#### Base Endpoint Example

```
wss://dialogflow-web-v2.gateway.dialogflow.cloud.ushakov.co
```

## Requests

### Detecting Intents

#### Request

```http
POST <BASE_ENDPOINT>
```

#### Request Variables

| Variable | Description |
|----------|-------------|
| BASE_ENDPOINT | Required, Endpoint of Agent |

#### Request Body

[DetectIntentRequest](https://cloud.google.com/dialogflow/docs/reference/rpc/google.cloud.dialogflow.v2#google.cloud.dialogflow.v2.DetectIntentRequest)

#### Response Body

[DetectIntentResponse](https://cloud.google.com/dialogflow/docs/reference/rest/v2beta1/DetectIntentResponse)

#### Example Request

Note: The examples are using NodeJS

Note: Dialogflow Gateway converts `session` into `projects/<Project ID>/agent/sessions/<Session ID>`. This behaviour is a subject to change

Multi-Agent scenario using wildcard subdomain on Dialogflow Gateway by Ushakov (Hosted)

```js
const WebSocket = require('ws')
const ws = new WebSocket('ws://app-of-the-day-9a9f6.gateway.dialogflow.cloud.ushakov.co')

ws.send(
  JSON.stringify({
    session: "123",
    queryInput: {
      text: {
        text: "Hello",
        languageCode: "en"
      }
    }
  })
)

ws.on('message', data => {
  console.log(data)
})

ws.on('close', (code, error) => {
  console.log(code, error)
})
```

#### Example Response

```json
{
  "responseId": "85c3d6bd-6f8c-4000-a1ec-fdd6f636b2e8",
  "queryResult": {
    "queryText": "Hello",
    "action": "input.welcome",
    "parameters": {},
    "allRequiredParamsPresent": true,
    "fulfillmentText": "Cannot display response in Dialogflow simulator. Please test on the Google Assistant simulator instead.",
    "fulfillmentMessages": [...],
    "webhookPayload": {...},
    "intent": {
      "name": "projects/app-of-the-day-9a9f6/agent/intents/1e00bd68-62f0-4b8e-9cdd-a81c19f73855",
      "displayName": "Default Welcome Intent"
    },
    "intentDetectionConfidence": 1,
    "diagnosticInfo": {
      "end_conversation": true,
      "webhook_latency_ms": 187
    },
    "languageCode": "en"
  },
  "webhookStatus": {
    "message": "Webhook execution successful"
  }
}
```

## Contact

If you have any questions or troubles regarding the API, please [contact us](https://ushakov.co/#contact)