---
title: "Configure API Gateway"
weight: 4
chapter: true
pre: " <b> 5.3.2. </b> "
---

---

## Introduction

API Gateway is the entry point for the Backend, allowing Frontend to invoke Lambda functions through HTTP requests. In this section, you will create a REST API with CORS support.

## Step 1: Create REST API

### Access API Gateway Console

1. AWS Console → Find **API Gateway**
2. Or access: [console.aws.amazon.com/apigateway](https://console.aws.amazon.com/apigateway)

### Select API Type

1. Press **Create API**
2. Select **REST API** (not REST API Private or HTTP API)

### Configure New API

| Setting | Value |
|---------|-------|
| **API type** | REST API |
| **Protocol** | REST |
| **Create new API** | New API |
| **API name** | `LunaGenZ-API` |
| **Description** | `API Gateway for LunaGenZ application` |
| **Endpoint type** | Regional |

> **💡 Regional vs Edge-Optimized:** Regional endpoint is better for Lambda in the same region. Edge-optimized is better for global users but has additional latency.

## Step 2: Create Resource

### Create `/calculate` resource

1. In the API dashboard, press **Create resource**

2. Configure:

| Setting | Value |
|---------|-------|
| **Resource name** | `calculate` |
| **Resource path** | `/calculate` |
| **Enable API Gateway CORS** | ✅ Yes |

3. Press **Create resource**

## Step 3: Create POST Method

### Add POST method

1. Select the `/calculate` resource you just created
2. Press **Create method**

3. Configure:

| Setting | Value |
|---------|-------|
| **HTTP method** | POST |
| **Integration type** | Lambda function |
| **Lambda proxy integration** | ✅ Lambda proxy integration |
| **Lambda function** | `LunaGenZ-Calculation` (select your region) |

4. Press **Create method**

### Enable CORS for Method

1. After creating POST method, press **Enable CORS**

2. Configure:

| Setting | Value |
|---------|-------|
| **Access-Control-Allow-Origin** | `*` |
| **Access-Control-Allow-Headers** | `Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token` |
| **Access-Control-Allow-Methods** | `POST,OPTIONS` |

3. Press **Enable CORS and replace existing CORS headers**

## Step 4: Add GET Method (Optional)

For quick testing, add GET method:

1. Resource `/calculate` → **Create method**
2. HTTP method: **GET**
3. Integration type: **Lambda function**
4. Lambda function: `LunaGenZ-Calculation`

> **⚠️ Note:** GET has no body, so Lambda code needs adjustment to receive query parameters.

## Step 5: Deploy API

### Create Stage

1. Press **Deploy API**

2. Configure deployment:

| Setting | Value |
|---------|-------|
| **Deployment stage** | New stage |
| **Stage name** | `prod` |
| **Stage description** | `Production stage` |
| **Deployment description** | `Initial deployment` |

3. Press **Deploy**

### Get Invoke URL

After deployment, you will see:

```
Invoke URL: https://abc123xyz.execute-api.ap-southeast-1.amazonaws.com/prod
```

> **📝 Remember this URL** - You will use it in Frontend code.

## Step 6: Update Frontend

### Update `app.js`

Add API URL to code:

```javascript
// LunaGenZ Frontend Configuration
const API_CONFIG = {
    // Replace with your URL
    API_URL: 'https://abc123xyz.execute-api.ap-southeast-1.amazonaws.com/prod/calculate',
    
    // Other settings
    TIMEOUT: 30000,
    RETRY_COUNT: 3
};

// Function to call API
async function calculateNumerology(name, birthdate) {
    try {
        const response = await fetch(API_CONFIG.API_URL, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                name: name,
                birthdate: birthdate
            })
        });
        
        if (!response.ok) {
            throw new Error('API request failed');
        }
        
        const data = await response.json();
        return data;
        
    } catch (error) {
        console.error('Error:', error);
        throw error;
    }
}
```

## Step 7: Test API

### Test using curl

```bash
curl -X POST \
  'https://abc123xyz.execute-api.ap-southeast-1.amazonaws.com/prod/calculate' \
  -H 'Content-Type: application/json' \
  -d '{"Name": "John Smith", "birthdate": "1990-05-15"}'
```

### Test using Postman

1. Method: POST
2. URL: API Gateway URL
3. Headers: `Content-Type: application/json`
4. Body:

```json
{
    "name": "John Smith",
    "birthdate": "1990-05-15"
}
```

### Test using Browser

Open browser and access:

```
https://abc123xyz.execute-api.ap-southeast-1.amazonaws.com/prod/calculate?name=John%20Smith&birthdate=1990-05-15
```

## Step 8: Configure Throttling (Optional)

### Set rate limit

1. API → **Settings**
2. Add throttling:

| Setting | Value |
|---------|-------|
| **Enable throttling** | ✅ |
| **Rate** | 100 requests per second |
| **Burst** | 50 requests |

## Step 9: Configure Usage Plans (Optional)

To limit and monitor API usage:

1. **Usage Plans** → **Create**
2. Configure:

| Setting | Value |
|---------|-------|
| **Plan name** | `Basic` |
| **Rate** | 1000 requests/day |
| **Quota** | 10000 requests/month |

3. Attach API and Stage

## Final API Structure

```
LunaGenZ-API
└── /calculate
    ├── POST → LunaGenZ-Calculation
    └── OPTIONS → CORS Preflight
```

## Summary

After this step, you have:

- ✅ REST API Gateway
- ✅ POST method calling Lambda
- ✅ CORS enabled
- ✅ API deployed to stage `prod`
- ✅ Invoke URL to use in Frontend

## Next Steps

Next, proceed to **Create Lambda PDF Generator** to generate PDF reports from calculation results.
