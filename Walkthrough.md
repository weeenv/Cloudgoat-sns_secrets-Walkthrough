This is a walkthrough of the Cloudgoat Scenario of sns_secrets

To initiate the CLoudgoat scenario, you can first run the following:

``` Cloudgoat create sns_secrets```

### Initial Credentials ###
When you run the cloudgoat command, you will be provided a set of AWS access key and secrets to start with, something like what you see below

```
sns_user_access_key_id = AKIXXXXXXXXXX3
sns_user_secret_access_key = NxxXXXXXXXXXXXWmLCVbkd0Ac88IHkx
```

### Configuring the AWS Credentials on the CMD ###

For me, I created a profile named sns, provide the access key and secrets that the generation of the scenarios provide, for the region, put us-east-1 and the format, you can put json
```
aws configure --profile sns
```

### SNS Initial User Identity ###

Run the command below to get the account identity and we get cg-sns-user-cgidrxqfnkkoal

```
aws sts get-caller-identity --profile sns
```

<img width="876" height="200" alt="image" src="https://github.com/user-attachments/assets/2f49d5f0-ef76-4235-a81e-5279f6495115" />

