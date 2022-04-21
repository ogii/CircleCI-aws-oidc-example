# aws-oidc-sample

This is a sample repository to demonstrate how to use the OIDC token provided by CircleCI to authenticate with AWS.

## Setting up an OpenID Connect identity provider (IdP) on AWS

Please follow the instructions on the following page:

https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html

### IdP Values

| Field  | Value |
| ------------- | ------------- |
| Provider URL  | https://oidc.circleci.com/org/{organization-id}  |
| Audience  | {organization-id}  |

You can find your organization-id under your organization settings page on CircleCI.

### Create an IAM role

You will need to create an IAM policy to use with `get-caller-identity` and for accessing S3. Here is a sample:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "sts:GetCallerIdentity",
                "s3:ListAllMyBuckets"
            ],
            "Resource": [
                "*"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "s3:CreateBucket",
                "s3:DeleteBucket",
                "s3:GetBucketTagging",
                "s3:PutBucketTagging",
                "s3:GetBucketPolicy",
                "s3:PutBucketPolicy",
                "s3:GetBucketAcl",
                "s3:PutBucketAcl",
                "s3:GetBucketCORS",
                "s3:PutBucketCORS",
                "s3:GetBucketWebsite",
                "s3:PutBucketWebsite",
                "s3:GetBucketVersioning",
                "s3:GetAccelerateConfiguration",
                "s3:GetBucketRequestPayment",
                "s3:GetBucketLogging",
                "s3:GetLifecycleConfiguration",
                "s3:GetReplicationConfiguration",
                "s3:GetEncryptionConfiguration",
                "s3:GetBucketObjectLockConfiguration",
                "s3:ListBucket",
                "s3:PutObject",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::circleci-oidc-test-aaron/*"
            ],
            "Effect": "Allow"
        }
    ]
}
```

### Create a CircleCI context
You will need to set at least one context for your job in order for the openid token to be available in the build.
In this sample repository, I am setting the following environment variables in a context named `aws-oidc`.

| Field  |
| ------------- |
| AWS_ROLE_ARN | 
| S3_TARGET  |

