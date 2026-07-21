---
title: "Create Lambda PDF Function"
weight: 4
chapter: true
pre: " <b> 5.4.1. </b> "
---

---

## Introduction

This section guides you through creating a Lambda function to generate PDF reports from numerology calculation results.

## Processing Flow

```
Lambda Calculation → Result → Lambda PDF Generator → Save to S3 → Return URL
```

## Step 1: Create IAM Role for PDF Lambda

### Create Role with S3 Permissions

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::lunagenz-reports-bucket/*"
        }
    ]
}
```

## Step 2: Create Lambda Function

### Configuration

| Setting | Value |
|---------|-------|
| **Function name** | `LunaGenZ-PDFGenerator` |
| **Runtime** | Node.js 20.x |
| **Timeout** | 30 seconds |

### PDF Generation Code

```javascript
const PDFDocument = require('pdfkit');

exports.handler = async (event) => {
    try {
        const { name, birthdate, results } = JSON.parse(event.body);
        
        // Create PDF document
        const doc = new PDFDocument();
        const chunks = [];
        
        doc.on('data', chunk => chunks.push(chunk));
        doc.on('end', () => {
            const pdfBuffer = Buffer.concat(chunks);
            // Save to S3 or return as base64
        });
        
        // Add content to PDF
        doc.fontSize(25).text('LunaGenZ Numerology Report', { align: 'center' });
        doc.moveDown();
        doc.fontSize(12).text(`Name: ${name}`);
        doc.text(`Birthdate: ${birthdate}`);
        doc.moveDown();
        
        // Add numerology results
        doc.fontSize(16).text('Your Numerology Numbers:', { underline: true });
        doc.moveDown();
        
        doc.fontSize(14).text(`Life Path Number: ${results.lifePathNumber}`);
        doc.text(`Expression Number: ${results.expressionNumber}`);
        doc.text(`Soul Urge Number: ${results.soulUrgeNumber}`);
        doc.text(`Personality Number: ${results.personalityNumber}`);
        doc.text(`Birthday Number: ${results.birthdayNumber}`);
        
        doc.moveDown();
        doc.fontSize(14).text('Interpretation:', { underline: true });
        doc.fontSize(12).text(results.interpretation);
        
        doc.end();
        
        return {
            statusCode: 200,
            body: JSON.stringify({ success: true })
        };
    } catch (error) {
        return {
            statusCode: 500,
            body: JSON.stringify({ error: error.message })
        };
    }
};
```

## Summary

After this step, you have:

- ✅ Lambda function to generate PDF reports
- ✅ Integration with S3 for storing reports
- ✅ Ready to integrate with main calculation flow

## Next Steps

Proceed to **Hosting Frontend** to deploy the frontend application.
