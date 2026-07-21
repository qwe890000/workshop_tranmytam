---
title: "Tạo Lambda PDF Generator"
weight: 5
chapter: true
pre: " <b> 5.4.1. </b> "
---

# Tạo Lambda PDF Generator

## Giới thiệu

Lambda function này chịu trách nhiệm tạo báo cáo PDF từ kết quả tính toán thần số học.

## Tạo Lambda cho PDF Generation

### Bước 1: Tạo Lambda Function mới
- Name: `LunaGenZ-PDFGenerator`
- Runtime: **Node.js 20.x**

### Bước 2: Cấu hình Lambda Layer
Để sử dụng Puppeteer (chuyển HTML sang PDF), bạn cần tạo Lambda Layer chứa Chrome và Puppeteer.

```bash
# Tạo thư mục layer
mkdir -p chrome-layer/nodejs
cd chrome-layer/nodejs

# Cài đặt puppeteer-core
npm init -y
npm install puppeteer-core
```

### Bước 3: Upload Layer lên AWS
- Nén thư mục `nodejs` thành `.zip`
- Upload lên Lambda Layer

### Bước 4: Lambda Function Code

```javascript
const puppeteer = require('puppeteer-core');
const S3 = require('@aws-sdk/client-s3');

const s3 = new S3.S3Client({});
const BUCKET = 'lunagenz-reports-bucket';

exports.handler = async (event) => {
    const { userId, calculationData } = JSON.parse(event.body);
    
    // Render HTML template with data
    const html = generatePDFHTML(calculationData);
    
    // Launch Chrome and create PDF
    const browser = await puppeteer.launch({
        executablePath: '/opt/chrome',
        args: ['--no-sandbox', '--disable-setuid-sandbox']
    });
    
    const page = await browser.newPage();
    await page.setContent(html);
    
    const pdfBuffer = await page.pdf({
        format: 'A4',
        printBackground: true
    });
    
    await browser.close();
    
    // Upload to S3
    const key = `reports/${userId}/${Date.now()}.pdf`;
    await s3.send(new S3.PutObjectCommand({
        Bucket: BUCKET,
        Key: key,
        Body: pdfBuffer,
        ContentType: 'application/pdf'
    }));
    
    return {
        statusCode: 200,
        body: JSON.stringify({
            downloadUrl: `https://${BUCKET}.s3.amazonaws.com/${key}`
        })
    };
};

function generatePDFHTML(data) {
    return `
    <html>
    <head>
        <style>
            body { font-family: Arial, sans-serif; padding: 40px; }
            h1 { color: #2c3e50; }
        </style>
    </head>
    <body>
        <h1>Numerology Report</h1>
        <p>Life Path Number: ${data.lifePathNumber}</p>
        <p>Expression Number: ${data.expressionNumber}</p>
        <p>Soul Urge Number: ${data.soulUrgeNumber}</p>
    </body>
    </html>`;
}
```

---

## Bước tiếp theo

Chuyển sang **Triển khai Frontend** để deploy frontend trên Amazon S3.
