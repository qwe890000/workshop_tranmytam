---
title: "Cấu hình API Gateway"
weight: 4
chapter: true
pre: " <b> 5.3.2. </b> "
---

# Cấu hình API Gateway

## Giới thiệu

API Gateway là điểm vào (entry point) cho Backend, cho phép Frontend gọi Lambda functions thông qua HTTP requests. Trong phần này, bạn sẽ tạo REST API với CORS support.

## Bước 1: Tạo REST API

### Truy cập API Gateway Console

1. AWS Console → Tìm **API Gateway**
2. Hoặc truy cập: [console.aws.amazon.com/apigateway](https://console.aws.amazon.com/apigateway)

### Chọn loại API

1. Nhấn **Create API**
2. Chọn **REST API** (không phải REST API Private hay HTTP API)

### Cấu hình API mới

| Setting | Value |
|---------|-------|
| **API type** | REST API |
| **Protocol** | REST |
| **Create new API** | New API |
| **API name** | `LunaGenZ-API` |
| **Description** | `API Gateway for LunaGenZ application` |
| **Endpoint type** | Regional |

> **💡 Regional vs Edge-Optimized:** Regional endpoint tốt hơn cho Lambda cùng region. Edge-optimized tốt cho global users nhưng có độ trễ thêm.

## Bước 2: Tạo Resource

### Tạo `/calculate` resource

1. Trong API dashboard, nhấn **Create resource**

2. Cấu hình:

| Setting | Value |
|---------|-------|
| **Resource name** | `calculate` |
| **Resource path** | `/calculate` |
| **Enable API Gateway CORS** | ✅ Yes |

3. Nhấn **Create resource**

## Bước 3: Tạo Method POST

### Thêm POST method

1. Chọn resource `/calculate` vừa tạo
2. Nhấn **Create method**

3. Cấu hình:

| Setting | Value |
|---------|-------|
| **HTTP method** | POST |
| **Integration type** | Lambda function |
| **Lambda proxy integration** | ✅ Lambda proxy integration |
| **Lambda function** | `LunaGenZ-Calculation` (chọn region của bạn) |

4. Nhấn **Create method**

### Enable CORS cho Method

1. Sau khi tạo POST method, nhấn **Enable CORS**

2. Cấu hình:

| Setting | Value |
|---------|-------|
| **Access-Control-Allow-Origin** | `*` |
| **Access-Control-Allow-Headers** | `Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token` |
| **Access-Control-Allow-Methods** | `POST,OPTIONS` |

3. Nhấn **Enable CORS and replace existing CORS headers**

## Bước 4: Thêm GET Method (Optional)

Để test nhanh, thêm GET method:

1. Resource `/calculate` → **Create method**
2. HTTP method: **GET**
3. Integration type: **Lambda function**
4. Lambda function: `LunaGenZ-Calculation`

> **⚠️ Lưu ý:** GET không có body, nên Lambda code cần điều chỉnh để nhận query parameters.

## Bước 5: Deploy API

### Tạo Stage

1. Nhấn **Deploy API**

2. Cấu hình deployment:

| Setting | Value |
|---------|-------|
| **Deployment stage** | New stage |
| **Stage name** | `prod` |
| **Stage description** | `Production stage` |
| **Deployment description** | `Initial deployment` |

3. Nhấn **Deploy**

### Lấy Invoke URL

Sau khi deploy, bạn sẽ thấy:

```
Invoke URL: https://abc123xyz.execute-api.ap-southeast-1.amazonaws.com/prod
```

> **📝 Ghi nhớ URL này** - Bạn sẽ dùng nó trong Frontend code.

## Bước 6: Cập nhật Frontend

### Cập nhật `app.js`

Thêm API URL vào code:

```javascript
// LunaGenZ Frontend Configuration
const API_CONFIG = {
    // Thay thế bằng URL của bạn
    API_URL: 'https://abc123xyz.execute-api.ap-southeast-1.amazonaws.com/prod/calculate',
    
    // Các settings khác
    TIMEOUT: 30000,
    RETRY_COUNT: 3
};

// Hàm gọi API
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

## Bước 7: Test API

### Test bằng curl

```bash
curl -X POST \
  'https://abc123xyz.execute-api.ap-southeast-1.amazonaws.com/prod/calculate' \
  -H 'Content-Type: application/json' \
  -d '{"name": "John Smith", "birthdate": "1990-05-15"}'
```

### Test bằng Postman

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

### Test bằng Browser

Mở trình duyệt và truy cập:

```
https://abc123xyz.execute-api.ap-southeast-1.amazonaws.com/prod/calculate?name=John%20Smith&birthdate=1990-05-15
```

## Bước 8: Cấu hình Throttling (Optional)

### Set rate limit

1. API → **Settings**
2. Thêm throttling:

| Setting | Value |
|---------|-------|
| **Enable throttling** | ✅ |
| **Rate** | 100 requests per second |
| **Burst** | 50 requests |

## Bước 9: Cấu hình Usage Plans (Optional)

Để giới hạn và theo dõi API usage:

1. **Usage Plans** → **Create**
2. Cấu hình:

| Setting | Value |
|---------|-------|
| **Plan name** | `Basic` |
| **Rate** | 1000 requests/day |
| **Quota** | 10000 requests/month |

3. Attach API và Stage

## Cấu trúc cuối cùng của API

```
LunaGenZ-API
└── /calculate
    ├── POST → LunaGenZ-Calculation
    └── OPTIONS → CORS Preflight
```

## Tóm tắt

Sau bước này, bạn đã có:

- ✅ REST API Gateway
- ✅ POST method gọi Lambda
- ✅ CORS enabled
- ✅ API deployed lên stage `prod`
- ✅ Invoke URL để sử dụng trong Frontend

## Các bước tiếp theo

Tiếp theo, chuyển sang phần **Tạo Lambda PDF Generator** để tạo báo cáo PDF từ kết quả tính toán.
