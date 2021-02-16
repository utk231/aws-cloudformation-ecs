# aws-cloudformation-ecs / staging-test

## Synopsis

The `aws-cloudformation-ecs / staging-test` is used for testing a version of Senzing hosted on the staging server.

## How to deploy without much thinking

1. [Login](https://awssenzingsso.awsapps.com/start)
   to AWS using your SSO account.
1. Choose the correct account.
   Example:
   `CFn_xxxxx` > `CFn-Deploy`
1. Click the "Management console" link.
1. Visit [AWS Cloudformation console](https://console.aws.amazon.com/cloudformation/home)
1. In **Stacks**
    1. In upper-right, click "Create Stack" > "With new resources (standard)"
1. In **Create Stack**
    1. In **Specify template**
        1. Select ":radio_button: Upload a template file".
        1. Click "Choose file" button.
        1. Select the [staging-test/cloudformation.yaml](https://github.com/Senzing/aws-cloudformation-ecs/blob/main/cloudformation/staging-test/cloudformation.yaml) file on your local system.
           Example: `~/senzing.git/aws-cloudformation-ecs/cloudformation/staging-test/cloudformation.yaml`
    1. In lower-right, click on "Next" button.
1. In **Specify stack details**
    1. In **Stack name**
        1. Start stack name with your initials.
           Example: `mjd-4001`
    1. In **Parameters**
        1. In **Senzing installation**
            1. Accept the End User Licence Agreement
    1. In **Testing**
        1. Enter the name of the package on the staging server for Yum to install.
    1. Other parameters are optional.
    1. In lower-right, click "Next" button.
1. In **Configure stack options**
    1. Ignore this warning:
        1. "Failed to retrieve sns topics"
    1. In **Permissions**
        1. First box: Choose "IAM role name"
        1. Second box: Choose "CFn-Service-Role"
    1. In lower-right, click "Next" button.
1. In **Review {stack-name}**
    1. Near the bottom, in **Capabilities**
        1. Check ":ballot_box_with_check: I acknowledge that AWS CloudFormation might create IAM resources."
    1. In lower-right, click "Create stack" button.

## Using deployment

1. Visit [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/home).
    1. Make sure correct AWS region is selected.
1. Wait until "{stack-name}" status is `CREATE_COMPLETE`.
    1. Senzing formation takes about 15 minutes to fully deploy.
    1. May have to hit the refresh button a few times to get updated information.
1. Click on "{stack-name}" stack
1. Click on "Outputs" tab
1. Click on URLs:
    1. UrlApiServerHeartbeat
    1. UrlJupyter
    1. UrlSwagger
    1. UrlWebApp
    1. UrlXterm
