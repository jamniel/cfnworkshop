# CloudFormation Workshop
This documents is to demostrate how to use AWS command line to manage CloudFormation

## Usage
1. Correctly configure your AWS credential in your terminal;
2. Clone templates to your project folder;

## Stack Management
### Deploy Resource

Example:
```bash
aws cloudformation deploy --stack-name $YourStackName --template-file $YourTemplate  --parameter-overrides file://$parameterPath --tags purpose=cfnworkshop --capabilities CAPABILITY_NAMED_IAM --region=us-east-1 --profile $Profile
```

### Delete Resource
```bash
aws cloudformation delete-stack --stack-name $YourStackName --region=us-east-1 --profile $Profile
```

## Stack Sets Management
### Create Resource
**Step 1 Create Stack Set**
```bash
aws cloudformation create-stack-set \
    --stack-set-name $stacksetName \
    --template-body file://$templatePath \
    --description "description" \
    --parameters file://$parametersPath \
    --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM \
    --tags Key=purpose,Value=workshop\
    --administration-role-arn $yourStackSetAdminRoleArn \
    --execution-role-name $yourStackSetExecutionRoleName \
    --region us-east-1 \
    --profile $Profile
```
**Step 2 Create Stack Instance**
```bash
aws cloudformation create-stack-instances \
    --stack-set-name $stacksetName \
    --accounts file://$accoutListFilePath \
    --regions $targetRegions \
    --operation-preferences FailureToleranceCount=0 \
    --region $stackSetRegion \
    --profile $Profile
```
### Update Resource
```bash
aws cloudformation update-stack-set \
    --stack-set-name $stacksetName \
    --template-body file://$templatePath \
    --parameters file://$parametersPath \
    --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM \
    --tags Key=purpose,Value=workshop \
    --administration-role-arn $yourStackSetAdminRoleArn \
    --execution-role-name $yourStackSetExecutionRoleName \
    --regions $targetRegions \
    --accounts file://$accoutListFilePath \  
    --operation-preferences FailureToleranceCount=0 \      
    --region $stackSetRegion \
    --profile $Profile
```

### Delete Resource
**Step 1 delete stack instance**
```bash
aws cloudformation delete-stack-instances \
    --stack-set-name $stacksetName \
    --accounts file://$accoutListFilePath \
    --regions $targetRegions \
    --no-retain-stacks \
    --profile $Profile
```

**Step 2 delete stack set**
```bash
aws cloudformation delete-stack-set \
    --stack-set-name $stacksetName \
    --region=$stackSetRegion \
    --profile $Profile
```

## Contributors

Chen Xiaodong (AWS)