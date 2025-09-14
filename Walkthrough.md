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


### List Roles in the account ###

After we have setup the credentials in AWS CMD, we can try to do some enumeration for access such as role and user policies.

```
aws iam list-roles --profile sns
```

<img width="878" height="57" alt="image" src="https://github.com/user-attachments/assets/eb765b41-0a79-463d-bf17-f740adf7f2fb" />

However we are unable to list the roles with our current account


### List Attached User Policies ###

```
aws iam list-attached-user-policies --user-name cg-sns-user-cgidrxqfnkkoal --profile sns
```

Similarly, we did not yield anything when we tried to enumerate for attached user policies

<img width="877" height="115" alt="image" src="https://github.com/user-attachments/assets/5ceb34de-ea41-4230-9342-bcf1e285b596" />

### Usage of Pacu ###

To facilitate our exploitation, we make use of Pacu

```
pacu
```

<img width="888" height="1185" alt="image" src="https://github.com/user-attachments/assets/0b48150a-6db6-4d0a-b2d3-137775b8ecdb" />

We can create a new session, which I use the name 'sns'

We continue by importing the sns profile credentials into pacu

```
import_keys sns
```

<img width="676" height="83" alt="image" src="https://github.com/user-attachments/assets/3fb1d483-ac98-46f6-9a5c-1d117ae61334" />


### iam enumerate permissions ###

We continue by first enumerating the available permissions to the different AWS services that our current user can do.

```
run iam__enum_permissions
```
<img width="881" height="398" alt="image" src="https://github.com/user-attachments/assets/5909bcd8-bbb2-4266-b169-43bda916d184" />

We have 11 permissions but to see it in details, we need to get Pacu's results by running 'whoami'

```
whoami
```

Below are the permissions that we are allowed to
```
"Permissions": {
    "Allow": {
      "sns:subscribe": {
        "Resources": [
          "*"
        ]
      },
      "iam:listgroupsforuser": {
        "Resources": [
          "*"
        ]
      },
      "iam:listattacheduserpolicies": {
        "Resources": [
          "*"
        ]
      },
      "iam:listuserpolicies": {
        "Resources": [
          "*"
        ]
      },
      "sns:receive": {
        "Resources": [
          "*"
        ]
      },
      "sns:gettopicattributes": {
        "Resources": [
          "*"
        ]
      },
      "iam:getuserpolicy": {
        "Resources": [
          "*"
        ]
      },
      "sns:listtopics": {
        "Resources": [
          "*"
        ]
      },
      "sns:listsubscriptionsbytopic": {
        "Resources": [
          "*"
        ]
      },
      "apigateway:get": {
        "Resources": [
          "*"
        ]
      }
    },
    "Deny": {
      "apigateway:get": {
        "Resources": [
          "arn:aws:apigateway:us-east-1::/restapis/*/resources/*/integration",
          "arn:aws:apigateway:us-east-1::/apikeys",
          "arn:aws:apigateway:us-east-1::/restapis/*/resources/*/methods/GET",
          "arn:aws:apigateway:us-east-1::/restapis/*/resources/*/methods/*/integration",
          "arn:aws:apigateway:us-east-1::/apikeys/*",
          "arn:aws:apigateway:us-east-1::/restapis/*/integration",
          "arn:aws:apigateway:us-east-1::/restapis/*/methods/GET"
        ]
      }
    }
  }
}
```






