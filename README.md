<!--
title: 'AWS Simple HTTP Endpoint example in NodeJS'
description: 'This template demonstrates how to make a simple HTTP API with Node.js running on AWS Lambda and API Gateway using the Serverless Framework.'
layout: Doc
framework: v3
platform: AWS
language: nodeJS
authorLink: 'https://github.com/serverless'
authorName: 'Serverless, inc.'
authorAvatar: 'https://avatars1.githubusercontent.com/u/13742415?s=200&v=4'
-->

# Serverless Framework Node HTTP API on AWS

A fully serverless microservices architecture for managing testimonials, built with AWS Lambda, API Gateway, DynamoDB, and deployed using CloudFormation and Serverless Framework.

## 🏗️ Architecture
```
┌──────────────┐
│   Client     │  (Postman / Web App)
└──────┬───────┘
       │
       │ HTTP Requests
       ▼
┌──────────────────┐
│  API Gateway     │  REST API Endpoints
│  (Regional)      │  - GET /hello
└──────┬───────────┘  - POST /kaamBharo
       │              - GET /kaamDikhao
       │              - PUT /kaamKhatamKaro
       ▼
┌───────────────────────────────────────┐
│         Lambda Functions              │
├───────────────────────────────────────┤
│  1. HelloLambdaFunction               │
│  2. KaamBharoLambdaFunction          │
│  3. KaamDikhaoLambdaFunction         │
│  4. KaamKhatamKaroLambdaFunction     │
└───────────┬───────────────────────────┘
            │
            │ SDK Calls
            ▼
┌───────────────────┐
│    DynamoDB       │  NoSQL Database
│  (On-Demand)      │  Table: Testimonials
└───────────────────┘

Supporting Services:
┌────────────┐  ┌──────────────┐  ┌─────────────┐
│     S3     │  │ CloudWatch   │  │ CloudForm.  │
│ (Artifacts)│  │   (Logs)     │  │    (IaC)    │
└────────────┘  └──────────────┘  └─────────────┘
```

## 🚀 Features

- ✅ Fully serverless architecture (zero server management)
- ✅ Infrastructure as Code using CloudFormation
- ✅ RESTful API with complete CRUD operations
- ✅ Auto-scaling based on traffic
- ✅ Pay-per-use pricing model
- ✅ High availability across multiple AZs
- ✅ Centralized logging with CloudWatch
- ✅ Secure IAM role-based access

## 🔧 Installation & Setup

### 1. Clone Repository
```bash
git clone <repository-url>
cd aws-node-http-api-project
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Install Serverless Framework
```bash
npm install -g serverless
```

### 4. Configure AWS CLI
```bash
aws configure
# Enter your AWS Access Key ID
# Enter your AWS Secret Access Key
# Enter default region (e.g., us-east-1)
# Enter default output format (json)
```

### 5. Review Configuration
Check `serverless.yml` file for:
- Service name
- Region settings
- Lambda function configurations
- DynamoDB table settings
- API Gateway configurations

## 🚀 Deployment

### Deploy to AWS
```bash
# Deploy to dev stage (default)
serverless deploy

# Or use shorthand
sls deploy

# Deploy to specific stage
sls deploy --stage prod

# Deploy with verbose output
sls deploy --verbose
```

### Expected Output
```
Deploying aws-node-http-api-project to stage dev (us-east-2)

✔ Service deployed to stack aws-node-http-api-project-dev (265s)

endpoints:
  GET - https://{api-id}.execute-api.us-east-2.amazonaws.com/
  POST - https://{api-id}.execute-api.us-east-2.amazonaws.com/kaam
  GET - https://{api-id}.execute-api.us-east-2.amazonaws.com/kaam
  PUT - https://{api-id}.execute-api.us-east-2.amazonaws.com/kaam/{id}

functions:
  hello: aws-node-http-api-project-dev-hello (14 MB)
  kaamBharo: aws-node-http-api-project-dev-kaamBharo (14 MB)
  kaamDikhao: aws-node-http-api-project-dev-kaamDikhao (14 MB)
  kaamKhatamKaro: aws-node-http-api-project-dev-kaamKhatamKaro (14 MB)
```

## 📡 API Endpoints

### 1. Health Check
```http
GET /
```
**Response:**
```json
{
  "message": "Hello from AWS Lambda!"
}
```

### 2. Add Testimonial (Create)
```http
POST /kaam
Content-Type: application/json

{
  "learnerName": "John Doe",
  "testimonial": "Great learning experience!",
  "rating": 5
}
```

### 3. Get All Testimonials (Read)
```http
GET /kaam
```
**Response:**
```json
{
  "testimonials": [
    {
      "id": "uuid-123",
      "learnerName": "John Doe",
      "testimonial": "Great learning experience!",
      "rating": 5,
      "timestamp": "2024-01-10T12:00:00Z"
    }
  ]
}
```

### 4. Delete Testimonial
```http
PUT /kaam/{id}
```

## 🧪 Testing with Postman

### Import Collection
1. Open Postman
2. Click Import
3. Use the API endpoint URLs from deployment output

### Test Scenarios

**Test 1: Health Check**
- Method: GET
- URL: `https://{api-id}.execute-api.region.amazonaws.com/`
- Expected: 200 OK with welcome message

**Test 2: Create Testimonial**
- Method: POST
- URL: `https://{api-id}.execute-api.region.amazonaws.com/kaam`
- Body: JSON with testimonial data
- Expected: 201 Created with confirmation

**Test 3: Retrieve Testimonials**
- Method: GET
- URL: `https://{api-id}.execute-api.region.amazonaws.com/kaam`
- Expected: 200 OK with array of testimonials

**Test 4: Delete Testimonial**
- Method: PUT
- URL: `https://{api-id}.execute-api.region.amazonaws.com/kaam/{id}`
- Expected: 200 OK with deletion confirmation

## 🔐 IAM Permissions

### Lambda Execution Role
```yaml
Policies:
  - AWSLambdaBasicExecutionRole
  - DynamoDBFullAccess (scoped to table)
  - CloudWatchLogsFullAccess
```

### DynamoDB Permissions
- dynamodb:PutItem
- dynamodb:GetItem
- dynamodb:Scan
- dynamodb:DeleteItem

## 📈 Monitoring & Logging

### CloudWatch Logs
```bash
# View logs for specific function
sls logs -f hello

# Tail logs in real-time
sls logs -f kaamBharo --tail

# View logs for specific time range
sls logs -f kaamDikhao --startTime 1h
```

## 💰 Cost Breakdown

**Lambda:**
- Free Tier: 1M requests/month, 400,000 GB-seconds
- After: $0.20 per 1M requests

**API Gateway:**
- Free Tier: 1M API calls/month (12 months)
- After: $3.50 per 1M requests

**DynamoDB:**
- Free Tier: 25 GB storage, 25 WCU, 25 RCU
- On-Demand: $1.25 per million write requests

**Estimated Monthly Cost (after free tier):**
- Low traffic (10K requests): <$1
- Medium traffic (100K requests): ~$5
- High traffic (1M requests): ~$25

## 📚 Key Learnings

1. **Serverless Architecture**
   - Eliminates server management overhead
   - Automatic scaling based on demand
   - Pay-per-use pricing model

2. **Infrastructure as Code**
   - CloudFormation provides reproducible deployments
   - Version control for infrastructure changes
   - Easy rollback capabilities

3. **API Design**
   - RESTful principles for clean API structure
   - Proper HTTP methods and status codes
   - Error handling and validation

4. **AWS Services Integration**
   - Lambda-DynamoDB integration patterns
   - API Gateway Lambda proxy integration
   - CloudWatch for observability

## 🚀 Future Enhancements

- [ ] Add authentication with AWS Cognito
- [ ] Implement API key management
- [ ] Add input validation schemas
- [ ] Set up multi-environment deployments (dev/staging/prod)
- [ ] Implement CI/CD pipeline with GitHub Actions
- [ ] Add API documentation with Swagger/OpenAPI
- [ ] Implement caching with API Gateway
- [ ] Add X-Ray tracing for debugging
- [ ] Create Lambda layers for shared code
- [ ] Implement DynamoDB streams for event processing

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

MIT License

## 📧 Contact

For questions or feedback:
- LinkedIn: https://www.linkedin.com/in/pavangupta29/
- Email: manipalstudentbca29@gmail.com
- GitHub: pavannn29

---

**⭐ If you found this project helpful, please give it a star!**

Built with ❤️ using AWS Serverless Services

To learn more about the capabilities of `serverless-offline`, please refer to its [GitHub repository](https://github.com/dherault/serverless-offline).
