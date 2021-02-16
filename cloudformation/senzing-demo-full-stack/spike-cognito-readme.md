# How to set up Cognito

AWS Cognito, is an Amazon managed service that provides authentication, authorization, and user management for web applications. To setup, we need to do the following steps.

1. Generate self signed certificate
1. Upload certificate to IAM
1. Include Cognito cloud formation code

## Generate Self Signed Certificates

To generate a self signed certificate

```console
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout privateKey.key -out certificate.crt
```

Convert the private key and cert into .pem encoded file

```console
openssl rsa -in privateKey.key -text > private.pem
openssl x509 -inform PEM -in certificate.crt > public.pem
```

## Upload certificate to IAM

You would need the aws cli and aws access keys to run the following command. After the running it, take down the Arn of the certificate. You will need it later for the cognito cloud formation code

```console
aws iam upload-server-certificate --server-certificate-name ExampleCertificate --certificate-body file://public.pem --private-key file://private.pem
```

## Include Cognito cloud formation code

Add the following to your cloudformation yaml file.

```yaml
UserPool:
  Type: 'AWS::Cognito::UserPool'
  Properties:
    AdminCreateUserConfig:
      AllowAdminCreateUserOnly: true # Disable self-registration
      InviteMessageTemplate:
        EmailSubject: !Sub '${AWS::StackName}: temporary password'
        EmailMessage: 'Use the username {username} and the temporary password {####} to log in for the first time.'
        SMSMessage: 'Use the username {username} and the temporary password {####} to log in for the first time.'
    AutoVerifiedAttributes:
    - email
    UsernameAttributes:
    - email
    Policies:
      PasswordPolicy: #secure password policy
        MinimumLength: 16
        RequireLowercase: true
        RequireNumbers: true
        RequireSymbols: true
        RequireUppercase: true
        TemporaryPasswordValidityDays: 21
UserPoolDomain: # Provides Cognito Login Page
  Type: 'AWS::Cognito::UserPoolDomain'
  Properties:
    UserPoolId: !Ref UserPool
    Domain: !Select [2, !Split ['/', !Ref 'AWS::StackId']] # Generates a unique domain name
UserPoolClient:
  Type: 'AWS::Cognito::UserPoolClient'
  Properties:
    AllowedOAuthFlows:
    - code # Required for ALB authentication
    AllowedOAuthFlowsUserPoolClient: true # Required for ALB authentication
    AllowedOAuthScopes:
    - openid
    CallbackURLs:
    - !Sub https://${LoadBalancer.DNSName}/oauth2/idpresponse # Redirects to the ALB
    GenerateSecret: true
    SupportedIdentityProviders: # Optional: add providers for identity federation
    - COGNITO
    UserPoolId: !Ref UserPool
```

Include this in your Cloudformation Parameters

```yaml
# the IAM server certificate ARN
CertificateArn:
  Type: String
```

Lastly add this to your load balancer's HTTPS Listener

```yaml
Certificates:
- CertificateArn: !Ref CertificateArn
DefaultActions: # 1. Authenticate against Cognito User Pool
- Type: 'authenticate-cognito'
  AuthenticateCognitoConfig:
    OnUnauthenticatedRequest: 'authenticate' # Redirect unauthenticated clients to Cognito login page
    Scope: 'openid'
    UserPoolArn: !GetAtt 'UserPool.Arn'
    UserPoolClientId: !Ref UserPoolClient
    UserPoolDomain: !Ref UserPoolDomain
  Order: 1
- Type: forward # 2. Forward request to target group (e.g., EC2 instances)
  TargetGroupArn: !Ref TargetGroup
  Order: 2
```