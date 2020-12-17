# Cloudformation:  senzing-demo-full-stack

## Abstract

The `senzing-demo-full-stack` demonstration 



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
1. **Example:** dbPassword4Me
1. **Default:** None

### DbUsername

1. **Synopsis:** The username used to access the AWS Aurora Postgres Serverless databases.
1. **Required:** Yes
1. **Type:** String
1. **Allowed pattern:** A letter followed by letters or numbers. Specifically: `[a-zA-Z][a-zA-Z0-9]*`
1. **Allowed values:** String of length 1 to 16 characters.
1. **Example:** user1234
1. **Default:** senzing

### RunSshd

1. **Synopsis:**
   Optionally, run a container that allows `ssh` and `scp` access.
   Can be used for debugging, copying files to the EFS, or the Senzing Exploratory Tools.
1. **Required:** Yes
1. **Type:** Boolean
1. **Allowed values:**  [ "Yes" | "No" ]
1. **Default:** No
1. **References:**
    1. [github.com/Senzing/docker-sshd](https://github.com/Senzing/docker-sshd)

### RunStreamProducer

1. **Synopsis:**
   Optionally, run a container that fetches JSON lines from a file and pushes them to the SQS queue.
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
1. **References:**
    1. [github.com/Senzing/stream-producer](https://github.com/Senzing/stream-producer)

### RunSwagger

1. **Synopsis:**
   Optionally, run a container that hosts the SwaggerUI for viewing the Senzing REST API OpenAPI document.
1. **Required:** Yes
1. **Type:** Boolean
1. **Allowed values:**  [ "Yes" | "No" ]
1. **Default:** No
1. **References:**
    1. [github.com/Senzing/senzing-rest-api-specification](https://github.com/Senzing/senzing-rest-api-specification).

### RunWebApp

1. **Synopsis:**
   Optionally, run a container that hosts the Senzing Entity Search Web App.
1. **Required:** Yes
1. **Type:** Boolean
1. **Allowed values:**  [ "Yes" | "No" ]
1. **Example:**
1. **Default:** Yes
1. **References:**
    1. [github.com/Senzing/entity-search-web-app](https://github.com/Senzing/entity-search-web-app)

### SenzingInputUrl

1. **Synopsis:**
   If using [RunStreamProducer](#runstreamproducer), supply the URL of a tar-gzipped file in JSON-lines format containing records to ingest into Senzing.
1. **Required:** Yes if running Stream Producer, otherwise no.
1. **Type:** String
1. **Allowed pattern:**  A URL starting with `http://` or `https://`.
1. **Example:** `https://www.example.com/my/records.json.gz`
1. **Default:** `https://public-read-access.s3.amazonaws.com/TestDataSets/test-dataset-100m.json.gz`

### SenzingLicenseAsBase64

1. **Synopsis:**
   To ingest more than 100,000 records, a Senzing license is required.
   A binary version of the Senzing license, `g2.lic`, is not usable as a parameter in the text entry field.
   Instead, a [Base64](https://en.wikipedia.org/wiki/Base64) representation of the information is needed.
   An example of how to produce base64 from `g2.lic` on Linux and macOS:

   ```console
   base64 /opt/senzing/etc/g2.lic
   ```

   Copy the entire output from the command and paste into the text entry field.
1. **Required:** Yes if ingesting more than 100,000 records, otherwise no.
1. **Type:** String
1. **Allowed pattern:** Empty or Base64 characters. Specifically `^$|[^-A-Za-z0-9+/=]|=[^=]|={3,}$`
1. **Allowed values:** Base64 encoded string
1. **Example:**

   ```console
   AQAAADgCAAAAAAAAU2VuemluZwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
   AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
   AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
   AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
   AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAARGVtbyBFeHBpcmVkAAAAAAAA
   AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAADIwMjAtMTItMTYA
   AAAAAAAAAAAARVZBTAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
   AAAAAAAAAAAAAAAAAAAAAFNUQU5EQVJEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
   AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKCGAQAAAAAAMTk3Ni0wMS0wMQAAAAAAAAAAAABN
   T05USExZAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
   AAAAAAAARkdIST5XYOZ90kbyAbU7wM7XvPCwq/FgORZIekwFMg8zi3tCD0V5+12q72aqk0E6JOct
   +cPAq/T50N5Pf5nvJZ6TaW3TzQbnH/z5f/ALsWLydE2DPNvq3HuAjkjZpg2h7mb4OUqorGxDI9RX
   TX8hPjzYrBfMdOgl1DlRBVG36WwdpB8AnSfaegbYU+U/vfof+ff6mJk8gzPg+OGPwg21/S6i2TT4
   RbTCSYP/TpfXyJGE6dbQWEC9rFhYuWq3mFF3z7zFEcmxpNfZuBtYsxni8P3sDZ706RA+wcQF7TVg
   giJoK03W8kd6mk3X+fvc4ARJo9RarYInsAvSHKlr1KpxeebuirfqgSz+uEW6pqOD1fV0oHnFncdf
   jV2k2CqmIfThB/ONQcn/4/EIlhdzXqxSlXAGz6C7ApHq6xUCdLILx/NfdUEypHIfyabrpXKOKOPx
   zekhGztEzB0gSJNebEa++EKxHDOc1Sc0YD9q9KvcaGSPTjlCJeaNhufg9Sz/iXZMP+d4Vkp+Bn6p
   mfUPG7tKharEoRChUNfRms8wVyNxmz6LRw5Uy14Dlodd0LyBQRB9Tx8FVYMh5AElwjbQOoDOIRvi
   IQIGsUNp/ZkP7PdBxc/b9o3rjUsZCzyCtP+jflZSqMenzXCsTI1Xay6On2wSVwQdJ1/2eIwKEfCF
   hj4DZlY5+jSo
   ```

1. **Default:** None

### SenzingRecordMax

1. **Synopsis:**
   When using [SenzingInputUrl](#senzinginputurl), this indicates the number of the last line that will be
   read from the file.
   It is used to limit the number of records ingested into Senzing.
1. **Required:** Yes if using [SenzingInputUrl](#senzinginputurl), otherwise no.
1. **Type:** Number
1. **Allowed pattern:** Numbers. Specifically: `[0-9]*`
1. **Allowed values:** 0 = Read entire file;  Any positive integer.
1. **Example:** 15000000
1. **Default:** 100000 - The largest number before a Senzing license is needed.

### SenzingRecordMin

1. **Synopsis:**
   When using [SenzingInputUrl](#senzinginputurl), this indicates the number of the first line that will be
   read from the file.
   Used to skip lines at the beginning of the file.
   It is handy if the beginning of the file has already been ingested into Senzing.
1. **Required:** Yes if using [SenzingInputUrl](#senzinginputurl), otherwise no.
1. **Type:** Number
1. **Allowed pattern:** Numbers. Specifically: `[0-9]*`
1. **Allowed values:** 0 = Read from beginning;  Any positive integer.
1. **Example:** 100000
1. **Default:** 0

### VpcAvailabilityZones

1. **Synopsis:**
   When using [VpcId](#vpcid), list VPC availability zones in which to create subnets.
   Two availability zones need to be specified.
   Anything after two will be ignored.
1. **Required:** Yes if using [VpcId](#vpcid), otherwise no.
1. **Type:** CommaDelimitedList
1. **Allowed pattern:** Comma-delimited list of VPC availability zones in which to create subnets.
1. **Example:** us-east-1a,us-east-1e
1. **Default:** None - default availability zones used based on
   [Fn::GetAZs](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-getavailabilityzones.html) and
   [AWS::Region](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html#cfn-pseudo-param-region)
1. **References:**
    1. [Regions and Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)

### VpcId

1. **Synopsis:**
   VPC Id of existing VPC.
   If not specified, a new VPC will be created.
1. **Required:** No
1. **Type:** String
1. **Allowed pattern:** `vpc-` followed by unique id. Specifically `^(?:vpc-[0-9a-f]{8}|vpc-[0-9a-f]{17}|)$`
1. **Example:** vpc-1a2b3c4d5e6f7g8h9
1. **Default:** None - a new VPC will be created
