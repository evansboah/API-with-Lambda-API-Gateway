
# 🛡️ UserRequestAPI — Secure User Data Retrieval with AWS Lambda & API Gateway

This project demonstrates how to build and deploy a RESTful API using **AWS Lambda**, **API Gateway**, and **DynamoDB**. It retrieves user-specific data based on a `userId` query parameter and is part of a broader initiative to master secure, scalable cloud-native architectures.

---

## 📦 Tech Stack

- **AWS Lambda** – Serverless compute for backend logic
- **API Gateway** – REST API management and routing
- **DynamoDB** – NoSQL database for user data
- **Swagger (OpenAPI)** – API documentation and versioning

---

## 🚀 Architecture Overview

```plaintext
Client → API Gateway → Lambda Function → DynamoDB
```

- **API Gateway** receives GET requests at `/users?userId=xyz`
- **Lambda** processes the request and queries DynamoDB
- **DynamoDB** returns user data (or an error if not found)
- **Response** is sent back through API Gateway to the client

---

## 🧠 Lambda Function Logic (Node.js)

```js
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DynamoDBDocumentClient, GetCommand } from "@aws-sdk/lib-dynamodb";

const ddbClient = new DynamoDBClient({ region: 'us-east-1' });
const ddb = DynamoDBDocumentClient.from(ddbClient);

export async function handler(event) {
  const userId = event.queryStringParameters?.userId;
  const params = { TableName: 'UserData', Key: { userId } };

  try {
    const command = new GetCommand(params);
    const { Item } = await ddb.send(command);

    if (Item) {
      return {
        statusCode: 200,
        body: JSON.stringify(Item),
        headers: { 'Content-Type': 'application/json' }
      };
    } else {
      return {
        statusCode: 404,
        body: JSON.stringify({ message: "No user data found" }),
        headers: { 'Content-Type': 'application/json' }
      };
    }
  } catch (error) {
    return {
      statusCode: 500,
      body: JSON.stringify({ message: "Internal Server Error", error }),
      headers: { 'Content-Type': 'application/json' }
    };
  }
}
```

---

## 📄 API Endpoint

| Method | Endpoint | Query Parameter | Response |
|--------|----------|------------------|----------|
| GET    | `/users` | `userId=abc123`  | JSON user data or error message |

- **Invoke URL:** `https://ijekus72ce.execute-api.us-east-1.amazonaws.com/prod/users`
- **Stage:** `prod`
- **Integration:** Lambda Proxy

---

## 📚 Swagger Documentation

The API is documented using OpenAPI v2.0 and includes:

- Endpoint structure
- Response formats
- Integration details
- Versioning metadata

> ✅ Available in `UserRequestAPI-prod-swagger-apigateway.json`

---

## 🧪 Project Reflection

This project deepened my understanding of:

- Lambda proxy integration and structured event handling
- RESTful API design and deployment stages
- Error handling and response formatting
- The importance of documentation for developer collaboration

---

## 🔮 Next Steps

- Provision and populate the DynamoDB `UserData` table
- Add IAM or Cognito-based authorization
- Extend API with POST and DELETE methods
- Integrate with frontend for full-stack testing

---



---

```

