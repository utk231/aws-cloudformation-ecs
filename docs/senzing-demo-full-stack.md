# Cloudformation:  senzing-demo-full-stack

## Abstract

Help for
[senzing-demo-full-stack](https://github.com/Senzing/aws-cloudformation-ecs/tree/main/cloudformation/senzing-demo-full-stack).

## Parameters

Technical information on AWS Cloudformation parameters can be seen at
[Parameters](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html).

### AcceptEula

1. **Synopsis:**
   To use the Senzing code, you must agree to the End User License Agreement (EULA).
   This step is intentionally tricky to ensure that you make a conscious effort to accept the EULA.
1. **Required:** Yes
1. **Type:** String
1. **Allowed values:** See [SENZING_ACCEPT_EULA](https://github.com/Senzing/knowledge-base/blob/master/lists/environment-variables.md#senzing_accept_eula).
1. **Default:** None

### DbPassword

1. **Synopsis:** The password used to access the AWS Aurora Postgres Serverless databases.
1. **Required:** Yes
1. **Type:** String
1. **Allowed pattern:** Letters and numbers. Specifically: `[a-zA-Z0-9]*`
1. **Allowed values:** String of length 8 to 41 characters.
1. **Example:**
1. **Default:** None

### DbUsername

1. **Synopsis:** The username used to access the AWS Aurora Postgres Serverless databases.
1. **Required:** Yes
1. **Type:** String
1. **Allowed pattern:** A letter followed by letters or numbers. Specifically: `[a-zA-Z][a-zA-Z0-9]*`
1. **Allowed values:** String of length 1 to 16 characters.
1. **Default:** senzing

### LogRetentionInDays

1. **Synopsis:**
   The number of days to retain the log events.
   For more information, see [AWS::Logs::LogGroup RetentionInDays](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-logs-loggroup.html#cfn-logs-loggroup-retentionindays).
1. **Required:** Yes
1. **Type:** Number
1. **Allowed values:** [ 1, 3, 5, 7, 14, 30, 60, 90, 120, 150, 180, 365, 400, 545, 731, 1827, 3653 ]
1. **Default:** 14

### RunSshd

1. **Synopsis:**
   Optionally, run a container that allows `ssh` and `scp` access.
   Can be used for debugging, copying files to the EFS, or the Senzing Exploratory Tools.
   More information at [github.com/Senzing/docker-sshd](https://github.com/Senzing/docker-sshd).
1. **Required:** Yes
1. **Type:** Boolean
1. **Allowed values:**  [ "Yes" | "No" ]
1. **Default:** No

### RunStreamProducer

1. **Synopsis:**
   Optionally, run a container that fetches JSON lines from a file and pushes them to the SQS queue.
   For more information, see [github.com/Senzing/stream-producer](https://github.com/Senzing/stream-producer).
   If "Yes" is chosen,
   [SenzingInputUrl](#senzinginputurl),
   [SenzingRecordMin](#senzingrecordmin),
   and
   [SenzingRecordMax](#senzingrecordmax)
   need to be specified.
1. **Required:** Yes
1. **Type:** Boolean
1. **Allowed values:**  [ "Yes" | "No" ]
1. **Default:** No

### RunSwagger

1. **Synopsis:**
   Optionally, run a container that hosts the SwaggerUI for viewing the Senzing REST API OpenAPI document.
   For more information, see [github.com/Senzing/senzing-rest-api-specification](https://github.com/Senzing/senzing-rest-api-specification).
1. **Required:** Yes
1. **Type:** Boolean
1. **Allowed values:**  [ "Yes" | "No" ]
1. **Default:** No

### RunWebApp

1. **Synopsis:**
   Optionally, run a container that hosts the Senzing Entity Search Web App.
   For more information, see [github.com/Senzing/entity-search-web-app](https://github.com/Senzing/entity-search-web-app).
1. **Required:** Yes
1. **Type:** Boolean
1. **Allowed values:**  [ "Yes" | "No" ]
1. **Example:**
1. **Default:** Yes

### SenzingInputUrl

1. **Synopsis:**
   If using [RunStreamProducer](#runstreamproducer), supply the URL of a tar-gzipped file in JSON-lines format containing records to ingest into Senzing.
1. **Required:** Yes if running Stream Producer, otherwise no.
1. **Type:** String
1. **Allowed pattern:**  A URL starting with `http://` or `https://`.
1. **Example:**
1. **Default:** `https://public-read-access.s3.amazonaws.com/TestDataSets/test-dataset-100m.json.gz`

### SenzingLicenseAsBase64

1. **Synopsis:**
   To ingest more than 100,000 records, a Senzing license is required.
   A binary version of the Senzing license, `g2.lic`, is not usable as a parameter in the text entry field.
   A [Base64](https://en.wikipedia.org/wiki/Base64) representation of the information is needed.
   An example of how to produce base64 from `g2.lic` on Linux and macOS:

   ```console
   base64 /opt/senzing/etc/g2.lic
   ```
   Copy the entire output from the command and paste into the text entry field.
1. **Required:** Only if ingesting more than 100,000 records.
1. **Type:** String
1. **Allowed pattern:** Base64 characters. Specifically `[^-A-Za-z0-9+/=]|=[^=]|={3,}$`
1. **Allowed values:** Base64 encoded string
1. **Example:**
1. **Default:** None

### SenzingRecordMax

1. **Synopsis:**
   When using [SenzingInputUrl](#senzinginputurl), this indicates the number of the last line that will be
   read from the file.
   It is used to limit the number of records ingested into Senzing.
1. **Required:** Yes, if using [SenzingInputUrl](#senzinginputurl).  Otherwise, no.
1. **Type:** Number
1. **Allowed pattern:** Numbers. Specifically: `[0-9]*`
1. **Allowed values:** 0 = Read entire file;  Any positive integer.
1. **Example:*
1. **Default:** 100000 - The largest number before a Senzing license is needed.

### SenzingRecordMin

1. **Synopsis:**
   When using [SenzingInputUrl](#senzinginputurl), this indicates the number of the first line that will be
   read from the file.
   It is used to skip lines at the beginning of the file.
   It is handy if the beginning of the file has already been ingested into Senzing.
1. **Required:** Yes, if using [SenzingInputUrl](#senzinginputurl).  Otherwise, no.
1. **Type:** Number
1. **Allowed pattern:** Numbers. Specifically: `[0-9]*`
1. **Allowed values:** 0 = Read from beginning;  Any positive integer.
1. **Example:**
1. **Default:** 0

### VpcAvailabilityZones

1. **Synopsis:**
   When using [VpcId](#vpcid), list VPC availability zones in which to create subnets.
   For list, see [Regions and Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)

1. **Required:** Yes, if using [VpcId](#vpcid).  Otherwise, no.
1. **Type:** CommaDelimitedList
1. **Allowed pattern:** Comma-delimited list of VPC availability zones in which to create subnets.
1. **Example:** us-east-1a,us-east-1e
1. **Default:** None - default availability zones used.
1. **References:**
    1. [Fn::GetAZs](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-getavailabilityzones.html)
    1. [AWS::Region](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html#cfn-pseudo-param-region)

### VpcId

1. **Synopsis:**
   VPC Id of existing VPC.
   If not specified, a new VPC will be created.
1. **Required:** No
1. **Type:** String
1. **Allowed pattern:** `vpc-` followed by id. Specifically `^(?:vpc-[0-9a-f]{8}|vpc-[0-9a-f]{17}|)$`
1. **Example:** vpc-0a1b2c3d4e5f6g7h8
1. **Default:** None - a new VPC will be created
