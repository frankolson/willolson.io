---
layout:     post
title:      CORS Proxy with AWS Lambda
# date:       2018-05-04 11:21:29
summary:    TBD.
categories: aws lambda cors typescript serverless
---

## Install
```
yarn add global serverless
# or: npm install serverless --global
```

## Create
```
serverless create -t aws-nodejs-typescript -n cors-proxy
cd cors-proxy
yarn add axios
yarn add serverless-offline --dev
```

## Configure
```typescript
// serverless.ts
import { Serverless } from 'serverless/aws';

const serverlessConfiguration: Serverless = {
  // ...
  // Add the serverless-offline plugin
  plugins: ['serverless-webpack', 'serverless-offline'],
  // ...
  functions: {
    get: {
      handler: 'handler.get',
      events: [
        {
          http: {
            method: 'get',
            path: '/{destination+}',
          }
        }
      ]
    }
  }
}

module.exports = serverlessConfiguration;
```

## Develop
```typescript
// handler.ts
import { APIGatewayProxyHandler, APIGatewayProxyResult } from 'aws-lambda';
import 'source-map-support/register';
import axios from 'axios';

export const get: APIGatewayProxyHandler = async (event, _context) => {
  if (destinatinoMissing(event.pathParameters)) {
    return missingUrlResponse();
  }

  const destination = event.pathParameters['destination']

  try {
    const result = await axios.get(destination)
    return {
      statusCode: 200,
      headers: {
        'content-type': result.headers['content-type'] || 'text/html; charset=utf-8',
        'Access-Control-Allow-Origin': '*',
        'Cache-Control': 'public, max-age=3600'
      },
      body: result.data
    }
  } catch (error) {
    console.error(error)
    throw error
  }
}

function destinatinoMissing(pathParameters: object): boolean {
  return (pathParameters === null) || (pathParameters['destination'] === null);
}

function missingUrlResponse(): APIGatewayProxyResult {
  return {
    statusCode: 400,
    body: null
  };
}
```

## Deploy
```
serverless offline
```
