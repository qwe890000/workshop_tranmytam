---
title: "Create Lambda PDF Generator"
weight: 5
chapter: true
pre: " <b> 5.4.1. </b> "
---

# Create Lambda PDF Generator

## Introduction

This Lambda function is responsible for generating PDF reports from numerology calculation results.

## Create Lambda for PDF Generation

### Step 1: Create New Lambda Function
- Name: `LunaGenZ-PDFGenerator`
- Runtime: **Node.js 20.x**

### Step 2: Configure Lambda Layer
To use Puppeteer (convert HTML to PDF), you need to create a Lambda Layer containing Chrome and Puppeteer.

```bash
# Create layer directory
mkdir -p chrome-layer/nodejs
cd chrome-layer/nodejs

# Install puppeteer-core
npm init -y
npm install puppeteer-core
```

### Step 3: Upload Layer to AWS
- Compress the `nodejs` folder into `.zip`
- Upload to Lambda Layer

### Step 4: Lambda Function Code

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

## Next Steps

Proceed to **Deploy Frontend** to deploy frontend on Amazon S3.
