# Quick Setup Guide - Amazon CodeGuru Reviewer Sample App

## Current Status ✅
- [x] AWS Step Functions state machine deployed
- [x] All Lambda functions created and working  
- [x] GitHub Actions workflow file added to repository
- [x] Workflow configured for `master` branch (your default branch)

## Next Steps to Complete Integration:

### Step 1: Create AWS Credentials for GitHub (Manual)

Since the automated script has issues, let's do this manually:

1. **Create IAM Policy:**
```bash
aws iam create-policy \
  --policy-name GitHubActionsStepFunctionsPolicy \
  --policy-document '{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "states:StartExecution"
            ],
            "Resource": "arn:aws:states:us-west-2:047786098634:stateMachine:security-scan-orchestration-dev"
        }
    ]
}'
```

2. **Create IAM User:**
```bash
aws iam create-user --user-name github-actions-security-scan
```

3. **Attach Policy:**
```bash
aws iam attach-user-policy \
  --user-name github-actions-security-scan \
  --policy-arn arn:aws:iam::047786098634:policy/GitHubActionsStepFunctionsPolicy
```

4. **Create Access Keys:**
```bash
aws iam create-access-key --user-name github-actions-security-scan
```

**Save the Access Key ID and Secret Access Key from the output!**

### Step 2: Configure GitHub Repository Secrets

1. **Go to your repository**: https://github.com/saurabh1531/amazon-codeguru-reviewer-sample-app

2. **Navigate to Settings:**
   - Click `Settings` tab
   - Go to `Secrets and variables` → `Actions`

3. **Add these secrets:**
   - `AWS_ACCESS_KEY_ID`: (from Step 1 output)
   - `AWS_SECRET_ACCESS_KEY`: (from Step 1 output)

### Step 3: Test the Integration

1. **Make a small change to your repository:**
   - Edit any file (like README.md)
   - Commit and push to `master` branch

2. **Monitor the execution:**
   - Go to GitHub → Actions tab to see the workflow running
   - Check AWS Console: https://us-west-2.console.aws.amazon.com/states/home?region=us-west-2

### Step 4: Verify Security Scanning

After pushing code, the workflow will:

1. ✅ Trigger on push to `master` branch
2. ✅ Start Step Functions execution
3. ✅ Run security scans (CodeGuru, Profiler, Inspector)
4. ✅ Analyze results with AI
5. ✅ Create tickets and send notifications
6. ✅ Store results in DynamoDB and S3

## Troubleshooting

**If workflow fails:**
- Check GitHub Actions logs for detailed error messages
- Verify AWS credentials are correctly set in GitHub secrets
- Ensure IAM permissions are properly configured

**If Step Functions fails:**
- Check CloudWatch logs for Lambda function errors
- Verify all Lambda functions are deployed and active

## Current Configuration

- **Repository**: amazon-codeguru-reviewer-sample-app
- **Branch**: master (configured)
- **State Machine**: security-scan-orchestration-dev
- **Region**: us-west-2
- **Trigger**: Push to master branch + Pull requests to master

## What Happens Next

Once you complete the setup and push code:
1. GitHub Actions will automatically trigger
2. Your code will be scanned for security vulnerabilities
3. Performance analysis will be performed
4. AI will analyze results and provide recommendations
5. Tickets will be created for any issues found
6. Notifications will be sent to configured channels

🎉 Your automated security scanning pipeline will be fully operational!