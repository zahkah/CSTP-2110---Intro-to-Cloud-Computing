# CSTP-2110---Intro-to-Cloud-Computing

# Design Document: Mental Health Support Chatbot

---

## Section 1 - Project Description

### 1.1 Project
**Mental Health Support Chatbot using AWS Cloud Services**

### 1.2 Description
A cloud-native chatbot solution built using Amazon Lex, Lambda, DynamoDB, and other AWS services to provide 24/7 mental health support, symptom assessments, and coping strategies to users through a conversational AI interface.

### 1.3 Revision History
| Date       | Comment                        | Author       |
|------------|--------------------------------|--------------|
| 2025-03-01 | Initial Draft                  | Project Lead |

---

## Section 2 - Overview

### 2.1 Purpose
This module implements the backend and frontend components of a mental health support chatbot that provides automated conversation and crisis support to users. Intended for project managers, developers, and QA testers.

### 2.2 Scope
The chatbot can provide real-time assistance, recommend resources, assess mood and stress levels, and suggest relaxation techniques. It integrates with AWS Lex, Lambda, DynamoDB, and is accessible via a web frontend.

### 2.3 Requirements

#### 2.3.1 Functional Requirements
- R1: The system shall allow users to communicate with the chatbot using natural language.
- R2: The system shall assess user inputs and provide appropriate mental health resources.
- R3: The system shall store user conversations securely.
- R4: The system shall detect crisis-related keywords and provide helpline contacts.

#### 2.3.2 Non-Functional Requirements
- Performance: Should support up to 500 concurrent users with minimal latency.
- Reliability: Should maintain 99.9% uptime with AWS-managed infrastructure.

#### 2.3.3 Technical Requirements
- AWS Lex for NLP.
- AWS Lambda for business logic.
- Amazon DynamoDB for data storage.
- AWS S3 and CloudFront for frontend hosting.

#### 2.3.4 Security Requirements
- All data should be encrypted in transit and at rest.
- IAM roles should restrict access based on least privilege.
- Use of Cognito for user session tracking (optional).

#### 2.3.5 Estimates
| # | Description                              | Hrs. Est. |
|---|------------------------------------------|-----------|
| 1 | Set up Lex Bot & Intents                 | 4         |
| 2 | Implement Lambda backend logic           | 6         |
| 3 | Connect Lex to Lambda                    | 2         |
| 4 | Create DynamoDB for chat logs            | 3         |
| 5 | React frontend for user interaction      | 6         |
| 6 | Integrate WebSockets/API Gateway         | 3         |
| 7 | Testing and QA                           | 4         |
|   | **TOTAL**                                | **28**    |

#### 2.3.6 Traceability Matrix
| SRS Requirement | SDD Module Reference |
|------------------|------------------------|
| Req 1            | 3.1, 5.1               |
| Req 2            | 3.1, 5.1.1             |
| Req 3            | 5.1                    |
| Req 4            | 3.2, 7.1               |

---

## Section 3 - System Architecture

### 3.1 Overview
The chatbot architecture is serverless, with Amazon Lex handling NLP, AWS Lambda processing logic, DynamoDB for data persistence, and API Gateway/WebSockets for communication.

### 3.2 Architectural Diagrams

#### 📦 Component Diagram (Text UML)
```
[User] --> [Web Frontend] --> [API Gateway] --> [Lambda Function] --> [Amazon Lex]
                                                       |
                                                       v
                                                 [DynamoDB Storage]
```

#### 🔄 Sequence Diagram (Text UML)
```
User -> Web Frontend: Enters message
Web Frontend -> API Gateway: Sends message
API Gateway -> Lambda Function: Trigger event
Lambda Function -> Amazon Lex: Analyze intent
Lambda Function -> DynamoDB: Store conversation
Lambda Function -> Web Frontend: Return response
Web Frontend -> User: Display reply
```

#### 🧩 Data Flow Diagram
```
User --> Web Interface --> API Gateway --> Lambda --> Lex (NLP) + DynamoDB (Logging)
```

#### 🚀 Deployment Diagram
```
[User Browser]
     |
     v
[S3 + CloudFront (Frontend)] ---> [API Gateway (WebSocket)] ---> [Lambda Function] ---> [Lex + DynamoDB]
```

---

## Section 4 - Data Dictionary
| Table         | Field     | Notes                                 | Type     |
|---------------|-----------|----------------------------------------|----------|
| ChatHistory   | userId    | Unique identifier per user             | String   |
|               | timestamp | Timestamp of the interaction           | String   |
|               | message   | User or bot message                    | String   |
|               | intent    | Identified intent from Lex             | String   |

---

## Section 5 - Data Design

### 5.1 Persistent/Static Data

#### 5.1.1 Dataset (ERD Overview)
```
Entity: User
- userId (PK)
- timestamp

Entity: Chat
- message
- intent
- response

Relationships:
User 1 ---- * Chat
```

---

## Section 6 - User Interface Design

### 6.1 UI Overview
A clean web interface with a chat window, input field, and chatbot responses.

### 6.2 UI Navigation Flow
```
Login Page → Chat Interface → Resource Suggestions → Feedback (optional)
```

### 6.3 Use Cases
- User enters a message
- Bot replies using Lex and Lambda-generated response
- UI updates with message history

---

## Section 7 - Testing

### 7.1 Test Plan
- **Objective**: Validate chatbot functionality & crisis detection
- **Scope**: Chat flows, intent resolution, AWS integrations
- **Test Environments**: Dev, UAT, Production

| Test Case         | Input                  | Expected Output            | Actual Output |
|------------------|------------------------|----------------------------|----------------|
| TC1               | "I feel anxious"       | Suggests coping technique | ✔️             |
| TC2               | "I'm in a crisis"      | Shows helpline info       | ✔️             |

---

## Section 8 - Monitoring
- CloudWatch Logs: Lambda function execution, errors
- Lex Console Metrics: Utterance success rates
- DynamoDB Metrics: Read/write throughput
- Alarms: High latency or error rate alerts

---

## Section 9 - Other Interfaces

### 9.1 API Gateway (WebSocket)
- Accepts user input via WebSocket
- Sends message to Lambda
- Returns response to frontend client

---

## Section 10 - Extra Design Features / Outstanding Issues
- Future expansion to mobile app
- Optional voice integration with Amazon Connect or Alexa

---

## Section 11 – References
- Amazon Lex Documentation
- AWS Lambda Best Practices
- Mental Health Commission of Canada
- Amazon DynamoDB Developer Guide

---

## Section 12 – Glossary
| Term        | Definition                           |
|-------------|--------------------------------------|
| Lex         | AWS chatbot/NLP service              |
| Lambda      | AWS serverless compute platform      |
| DynamoDB    | NoSQL database from AWS              |
| Bedrock     | AWS service for generative AI        |
| IAM         | Identity and Access Management       |

