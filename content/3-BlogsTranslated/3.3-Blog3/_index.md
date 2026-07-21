---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

---

## Building LunaGENZ – Personalized Numerology System on Serverless AWS with GenAI

Hello everyone, after some time of self-learning and grinding, our team has started working on the final project: LunaGENZ – a system that uses AI to interpret personalized numerology.

I wrote this article not to show off, but to document the architecture decision-making process, along with the mistakes we encountered – hoping it will be useful for anyone doing a similar project, and I really hope to receive feedback from experienced people so our team can learn more.

## 1. Why Choose Serverless Architecture

The initial goal was to avoid managing servers ourselves, while keeping costs appropriate for a student project without stable revenue. The team chose:

- Frontend: Next.js, deployed via AWS Amplify
- Backend: API Gateway + AWS Lambda
- Database: Amazon DynamoDB (On-demand mode)

The pay-per-actual-usage model helps infrastructure costs during the testing phase be very low, suitable for a product that doesn't have real users yet. This is appropriate for our team's specific problem at this stage, not a general conclusion for all projects — when traffic increases, the serverless model also has its own cost trade-offs that the team is still learning more about.

## 2. Timeout Problem and How to Handle It with Amazon SQS

Initially, the system called AI to generate content and export PDF in the same request. Because these steps took time, the system continuously hit timeout errors due to API Gateway's 29-second limit.

How the team handled it: split the processing flow using Amazon SQS. The user request only pushes a message to the queue and returns a "processing" response. Another Lambda running in the background will take the message, call AI, create PDF, then send via Amazon SES to the user's email. This method helped the system no longer have timeouts and reduced the risk of data loss when handling errors.

## 3. Choosing the AI Model

The team uses Claude 3 Haiku via Amazon Bedrock instead of larger models. The reason is that our Vietnamese text interpretation problem doesn't require overly complex reasoning, so a smaller, faster-responding, lower-cost model is a more reasonable choice compared to using the most powerful model possible.

## 4. Security and CI/CD

- The payment flow is separated into an independent Webhook Handler, separate from the AI interpretation part.
- Sensitive keys are stored in AWS Secrets Manager, not hardcoded in code.
- The entire frontend and backend are automatically deployed via GitLab CI/CD. Error and performance monitoring via CloudWatch.

## Some Things My Team and I Learned

This is the team's first time applying serverless architecture at a relatively complete level, so there are certainly many points that haven't been optimized yet – for example, error handling when background Lambda processing fails, or how to control costs when scaling are still problems the team hasn't tested at large scale.

Since it's also our first time doing a project that involves outside factors, we've also made quite a few mistakes. I hope you all will help contribute so we can do better. Thank you everyone for taking the time to read this article.
