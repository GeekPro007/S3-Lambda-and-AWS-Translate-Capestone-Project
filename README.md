# CloudLingo - Serverless Translation Service
 
## Project Overview
 
CloudLingo is a serverless translation service built on AWS that automatically translates JSON documents using AWS Translate. The service is designed as an Infrastructure-as-Code (IaC) solution using Terraform, demonstrating cloud-native architecture and DevOps best practices.
 
## Architecture
 
```
┌─────────────┐        ┌─────────────┐        ┌──────────────┐
│   User      │───────>│  S3 Request │───────>│    Lambda    │
│  Uploads    │        │   Bucket    │ Event  │   Function   │
│  JSON File  │        └─────────────┘        └──────┬───────┘
└─────────────┘                                      │
                                                     │ Translate
                                              ┌──────▼────────┐
                                              │ AWS Translate │
                                              └──────┬────────┘
                                                     │
┌─────────────┐        ┌─────────────┐              │
│   User      │<───────│ S3 Response │<─────────────┘
│  Downloads  │        │   Bucket    │   Store
│ Translation │        └─────────────┘  Result
└─────────────┘
```
 
### Components
 
1. **S3 Request Bucket**: Receives JSON files containing text to translate
2. **Lambda Function**: Processes translation requests automatically when files are uploaded
3. **AWS Translate**: Performs the actual language translation
4. **S3 Response Bucket**: Stores translated JSON files with timestamps
5. **IAM Roles**: Manages permissions for Lambda to access S3 and Translate services
 
## Features
 
- **Automated Translation**: Files uploaded to S3 automatically trigger translation
- **Multi-Language Support**: Supports 16+ languages including English, Spanish, French, German, Italian, Portuguese, Dutch, Polish, Russian, Japanese, Korean, Chinese, Arabic, Hindi, Hebrew, and Turkish
- **Secure Storage**: S3 buckets encrypted with AES256 and public access blocked
- **Cost Optimized**: Designed to operate within AWS Free Tier limits
- **Error Handling**: Comprehensive validation and error messages
- **File Management**: Automatic cleanup with 7-day lifecycle policies
 
## File Structure
 
```
Cap-stone-Project/
├── main.tf                 # AWS provider configuration
├── s3.tf                   # S3 buckets with security settings
├── lambda.tf               # Lambda function and triggers
├── iam.tf                  # IAM roles and policies
├── outputs.tf              # Terraform outputs
├── lambda_function.py      # Translation logic
├── README.md              # Project documentation
└── tests/                 # Sample test files
    ├── sample_translation.json
    └── sample_multi_language.json
```
 
## Quick Start
 
1. **Prerequisites**
   - AWS Account
   - Terraform installed
   - AWS CLI configured
 
2. **Deploy Infrastructure**
   ```bash
   terraform init
   terraform apply
   ```
 
3. **Test Translation**
   ```bash
   aws s3 cp tests/sample_translation.json s3://capstone-project-request-bucket-azubi/
   ```
 
4. **Check Results**
   ```bash
   aws s3 ls s3://capstone-project-response-bucket-azubi/
   ```
 
For detailed deployment instructions, see [guide.md](./guide.md).
 
## Input Format
 
JSON files must contain these required fields:
 
```json
{
  "text": "Text to translate",
  "source_language": "en",
  "target_language": "es"
}
```
 
### Supported Language Codes
 
- `en` - English
- `es` - Spanish
- `fr` - French
- `de` - German
- `it` - Italian
- `pt` - Portuguese
- `nl` - Dutch
- `pl` - Polish
- `ru` - Russian
- `ja` - Japanese
- `ko` - Korean
- `zh` - Chinese
- `ar` - Arabic
- `hi` - Hindi
- `he` - Hebrew
- `tr` - Turkish
 
## Output Format
 
The service generates translated JSON files with the following structure:
 
```json
{
  "original_text": "Original text",
  "translated_text": "Translated text",
  "source_language": "en",
  "target_language": "es",
  "translation_timestamp": "2024-01-15T10:30:00",
  "original_file": "input.json"
}
```
 
## Security Features
 
- **Encryption**: All S3 buckets use AES256 server-side encryption
- **Access Control**: Public access blocked on all buckets
- **IAM Policies**: Least-privilege access for Lambda function
- **Input Validation**: File size limits and content validation
 
## Cost Optimization
 
The service is designed to operate within AWS Free Tier limits:
 
- **Lambda**: 1M requests + 400,000 GB-seconds/month
- **S3**: 5GB storage + 20,000 GET + 2,000 PUT requests
- **Translate**: 2M characters/month (first 12 months)
- **Automatic Cleanup**: 7-day lifecycle policies on both buckets
 
## Limitations
 
- Maximum file size: 1MB
- Maximum text length: 5,000 characters per request
- Files automatically deleted after 7 days
 
## Troubleshooting
 
Common issues and solutions:
 
1. **Lambda not triggering**: Check S3 event notifications
2. **Translation errors**: Verify language codes and text format
3. **Permission errors**: Review IAM role policies
4. **Large files**: Split text into smaller chunks
 
## Clean Up
 
To remove all resources and avoid charges:
 
```bash
terraform destroy
```
 
## Technologies Used
 
- **Infrastructure**: Terraform
- **Cloud Provider**: AWS
- **Compute**: AWS Lambda (Python 3.9)
- **Storage**: Amazon S3
- **Translation**: AWS Translate
- **Language**: Python with Boto3 SDK
 
## Project Status
 
This project was developed as part of the AWS Cloud Intensive Course Cohort 2 - Phase 2 Capstone Project. It demonstrates proficiency in:
 
- Infrastructure as Code (IaC)
- Serverless Architecture
- AWS Services Integration
- Security Best Practices
- Cost Optimization
 
## License
 
This project is part of an educational program and is intended for learning purposes.
 
## Support
 
For issues or questions, please refer to the [deployment guide](./guide.md) or check the troubleshooting section.
 
