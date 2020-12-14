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
1. **Default:** Yes

### SenzingInputUrl

1. **Synopsis:**
   If using [RunStreamProducer](#runstreamproducer), supply the URL of a tar-gzipped file in JSON-lines format containing records to ingest into Senzing.
1. **Required:** Yes if running Stream Producer, otherwise no.
1. **Type:** String
1. **Allowed pattern:**  A URL starting with `http://` or `https://`.
1. **Default:** `https://public-read-access.s3.amazonaws.com/TestDataSets/test-dataset-100m.json.gz`

### SenzingLicenseAsBase64

1. **Synopsis:**
1. **Required:**
1. **Type:**
1. **Allowed pattern:**
1. **Allowed values:**
1. **Default:**

### SenzingRecordMax

1. **Synopsis:**
1. **Required:**
1. **Type:**
1. **Allowed pattern:**
1. **Allowed values:**
1. **Default:**

### SenzingRecordMin

1. **Synopsis:**
1. **Required:**
1. **Type:**
1. **Allowed pattern:**
1. **Allowed values:**
1. **Default:**

### VpcAvailabilityZones

1. **Synopsis:**
1. **Required:**
1. **Type:**
1. **Allowed pattern:**
1. **Allowed values:**
1. **Default:**

### VpcId

1. **Synopsis:**
1. **Required:**
1. **Type:**
1. **Allowed pattern:**
1. **Allowed values:**
1. **Default:**
