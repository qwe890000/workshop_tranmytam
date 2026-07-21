---
title: "Create Lambda Function"
weight: 3
chapter: true
pre: " <b> 5.3.1. </b> "
---

---

## Introduction

Lambda Calculation is the function that processes numerology calculation logic. This is the heart of the LunaGenZ application, where complex calculations are performed to provide meaningful numbers.

## Step 1: Create IAM Role for Lambda

Before creating the Lambda function, you need to create an IAM Role to grant permissions.

### Access IAM Console

1. AWS Console → Find **IAM**
2. Select **Roles** → **Create role**

### Configure Role

| Setting | Value |
|---------|-------|
| **Trusted entity type** | AWS service |
| **Use case** | Lambda |
| **Permissions policies** | `AWSLambdaBasicExecutionRole` |

### Add Inline Policy (if you need S3 access)

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::lunagenz-reports-bucket/*"
        }
    ]
}
```

## Step 2: Create Lambda Function

### Access Lambda Console

1. AWS Console → Find **Lambda**
2. Press **Create function**

### Basic Function Configuration

| Setting | Value |
|---------|-------|
| **Function name** | `LunaGenZ-Calculation` |
| **Runtime** | Node.js 20.x |
| **Architecture** | x86_64 |
| **Permissions** | Use existing role → select the role you created |

### Advanced Settings (Optional)

| Setting | Value |
|---------|-------|
| **Description** | Calculate numerology numbers |
| **Memory** | 128 MB |
| **Timeout** | 10 seconds |
| **VPC** | No VPC (for public access) |

## Step 3: Write Lambda Code

Replace the default code with the following:

```javascript
// LunaGenZ-Calculation Lambda Function
// Calculates numerology numbers based on name and birthdate

exports.handler = async (event) => {
    try {
        // Parse request body
        const { name, birthdate } = JSON.parse(event.body);
        
        // Validate input
        if (!name || !birthdate) {
            return {
                statusCode: 400,
                body: JSON.stringify({
                    error: 'Missing required fields: name and birthdate'
                })
            };
        }
        
        // Calculate numerology numbers
        const lifePathNumber = calculateLifePath(birthdate);
        const expressionNumber = calculateExpression(name);
        const soulUrgeNumber = calculateSoulUrge(name);
        const personalityNumber = calculatePersonality(name);
        const birthdayNumber = calculateBirthday(birthdate);
        
        // Return results
        return {
            statusCode: 200,
            headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*'
            },
            body: JSON.stringify({
                success: true,
                data: {
                    lifePathNumber,
                    expressionNumber,
                    soulUrgeNumber,
                    personalityNumber,
                    birthdayNumber,
                    interpretation: getInterpretation(lifePathNumber)
                }
            })
        };
        
    } catch (error) {
        return {
            statusCode: 500,
            body: JSON.stringify({
                error: 'Internal server error',
                message: error.message
            })
        };
    }
};

// Calculate digit sum with reduction
function digitSum(num, reduce = true) {
    let sum = String(num).split('').reduce((acc, digit) => {
        return acc + parseInt(digit, 10);
    }, 0);
    
    if (reduce && sum > 9 && sum !== 11 && sum !== 22 && sum !== 33) {
        return digitSum(sum);
    }
    return sum;
}

// Life Path Number from birthdate (YYYY-MM-DD)
function calculateLifePath(birthdate) {
    // Remove non-digits
    const digits = birthdate.replace(/\D/g, '');
    return digitSum(digits);
}

// Expression Number from full name
function calculateExpression(name) {
    // Map letters to numbers (Chaldean or Pythagorean)
    const letterValues = {
        'A': 1, 'B': 2, 'C': 3, 'D': 4, 'E': 5, 'F': 6, 'G': 7, 'H': 8, 'I': 9,
        'J': 1, 'K': 2, 'L': 3, 'M': 4, 'N': 5, 'O': 6, 'P': 7, 'Q': 8, 'R': 9,
        'S': 1, 'T': 2, 'U': 3, 'V': 4, 'W': 5, 'X': 6, 'Y': 7, 'Z': 8
    };
    
    const sum = name.toUpperCase().split('').reduce((acc, char) => {
        return acc + (letterValues[char] || 0);
    }, 0);
    
    return digitSum(sum);
}

// Soul Urge Number (vowels only)
function calculateSoulUrge(name) {
    const vowels = 'AEIOU';
    const letterValues = {
        'A': 1, 'E': 5, 'I': 9, 'O': 6, 'U': 3
    };
    
    const sum = name.toUpperCase().split('').reduce((acc, char) => {
        if (vowels.includes(char)) {
            return acc + (letterValues[char] || 0);
        }
        return acc;
    }, 0);
    
    return digitSum(sum);
}

// Personality Number (consonants only)
function calculatePersonality(name) {
    const vowels = 'AEIOU';
    const letterValues = {
        'A': 1, 'B': 2, 'C': 3, 'D': 4, 'F': 6, 'G': 7, 'H': 8, 'J': 1, 'K': 2,
        'L': 3, 'M': 4, 'N': 5, 'P': 7, 'Q': 8, 'R': 9, 'S': 1, 'T': 2, 'V': 4,
        'W': 5, 'X': 6, 'Y': 7, 'Z': 8
    };
    
    const sum = name.toUpperCase().split('').reduce((acc, char) => {
        if (!vowels.includes(char) && letterValues[char]) {
            return acc + letterValues[char];
        }
        return acc;
    }, 0);
    
    return digitSum(sum);
}

// Birthday Number (day of month)
function calculateBirthday(birthdate) {
    const parts = birthdate.split('-');
    const day = parseInt(parts[2], 10);
    return digitSum(day);
}

// Get interpretation for Life Path Number
function getInterpretation(num) {
    const interpretations = {
        1: "Leader, independent, original, innovative",
        2: "Diplomat, cooperative, peacemaker, mediator",
        3: "Creative, expressive, social, artistic",
        4: "Builder, practical, hardworking, organizer",
        5: "Adventurer, freedom seeker, versatile, curious",
        6: "Nurturer, responsible, caring, harmonious",
        7: "Seeker, analytical, spiritual, introspective",
        8: "Achiever, material success, authority, power",
        9: "Humanitarian, compassionate, universal thinker"
    };
    return interpretations[num] || interpretations[1];
}
```

### Save and Deploy

1. Press **Deploy** to update code

## Step 4: Test Lambda Function

### Create Test Event

1. Press **Test** tab
2. Press **Configure test events** → **Create new event**

3. Configure test event:

| Setting | Value |
|---------|-------|
| **Event name** | `test-calculation` |
| **Event JSON** | See below |

```json
{
  "body": "{\"name\": \"John Smith\", \"birthdate\": \"1990-05-15\"}"
}
```

4. Press **Save** → **Test**

### View Results

If successful, you will see:

```json
{
  "statusCode": 200,
  "body": "{\"success\":true,\"data\":{\"lifePathNumber\":7,\"expressionNumber\":6,\"soulUrgeNumber\":7,\"personalityNumber\":5,\"birthdayNumber\":6,\"interpretation\":\"Seeker, analytical, spiritual, introspective\"}}"
}
```

## Step 5: Configure Environment Variables (Optional)

1. Tab **Configuration** → **Environment variables**
2. Press **Edit**
3. Add variables:

| Key | Value | Description |
|-----|-------|------------|
| `LOG_LEVEL` | `info` | Logging level |
| `MAX_NAME_LENGTH` | `100` | Name length limit |

## Step 6: Monitoring

### View Logs in CloudWatch

1. Tab **Monitor** → **View CloudWatch logs**

2. CloudWatch Console will open with log streams

### Configure Alerts

1. **CloudWatch** → **Alarms** → **Create alarm**
2. Select metric: `Errors` for Lambda function
3. Threshold: > 0
4. SNS topic to receive notifications

## Summary

After this step, you have:

- ✅ IAM Role with required permissions
- ✅ Lambda Function `LunaGenZ-Calculation`
- ✅ Numerology calculation code
- ✅ Test event and successful results
- ✅ Monitoring configuration

## Next Steps

Next, proceed to **API Gateway** section to create REST API endpoint to invoke Lambda.
