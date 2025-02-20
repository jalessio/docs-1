import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Lambda

Deploy a Lambda on AWS.

<a href="https://github.com/gruntwork-io/terraform-aws-service-catalog/tree/master/modules/services/lambda" className="link-button">View on GitHub</a>

### Reference

<Tabs>
<TabItem value="inputs" label="Inputs" default>

<a name="alarm_sns_topic_arns" className="snap-top"></a>

* [**`alarm_sns_topic_arns`**](#alarm_sns_topic_arns) &mdash; A list of SNS topic ARNs to notify when the lambda alarms change to ALARM, OK, or [`INSUFFICIENT_DATA`](#INSUFFICIENT_DATA) state

<a name="assume_role_policy" className="snap-top"></a>

* [**`assume_role_policy`**](#assume_role_policy) &mdash; A custom assume role policy for the IAM role for this Lambda function. If not set, the default is a policy that allows the Lambda service to assume the IAM role, which is what most users will need. However, you can use this variable to override the policy for special cases, such as using a Lambda function to rotate AWS Secrets Manager secrets.

<a name="command" className="snap-top"></a>

* [**`command`**](#command) &mdash; The CMD for the docker image. Only used if you specify a Docker image via [`image_uri`](#image_uri).

<a name="comparison_operator" className="snap-top"></a>

* [**`comparison_operator`**](#comparison_operator) &mdash; The arithmetic operation to use when comparing the specified Statistic and Threshold. The specified Statistic value is used as the first operand. Either of the following is supported: `GreaterThanOrEqualToThreshold`, `GreaterThanThreshold`, `LessThanThreshold`, `LessThanOrEqualToThreshold`. Additionally, the values `LessThanLowerOrGreaterThanUpperThreshold`, `LessThanLowerThreshold`, and `GreaterThanUpperThreshold` are used only for alarms based on anomaly detection models.

<a name="create_resources" className="snap-top"></a>

* [**`create_resources`**](#create_resources) &mdash; Set to false to have this module skip creating resources. This weird parameter exists solely because Terraform does not support conditional modules. Therefore, this is a hack to allow you to conditionally decide if this module should create anything or not.

<a name="datapoints_to_alarm" className="snap-top"></a>

* [**`datapoints_to_alarm`**](#datapoints_to_alarm) &mdash; The number of datapoints that must be breaching to trigger the alarm.

<a name="dead_letter_target_arn" className="snap-top"></a>

* [**`dead_letter_target_arn`**](#dead_letter_target_arn) &mdash; The ARN of an SNS topic or an SQS queue to notify when invocation of a Lambda function fails. If this option is used, you must grant this function's IAM role (the ID is outputted as [`iam_role_id`](#iam_role_id)) access to write to the target object, which means allowing either the sns:Publish or sqs:SendMessage action on this ARN, depending on which service is targeted.

<a name="description" className="snap-top"></a>

* [**`description`**](#description) &mdash; A description of what the Lambda function does.

<a name="enable_versioning" className="snap-top"></a>

* [**`enable_versioning`**](#enable_versioning) &mdash; Set to true to enable versioning for this Lambda function. This allows you to use aliases to refer to execute different versions of the function in different environments. Note that an alternative way to run Lambda functions in multiple environments is to version your Terraform code.

<a name="entry_point" className="snap-top"></a>

* [**`entry_point`**](#entry_point) &mdash; The ENTRYPOINT for the docker image. Only used if you specify a Docker image via [`image_uri`](#image_uri).

<a name="environment_variables" className="snap-top"></a>

* [**`environment_variables`**](#environment_variables) &mdash; A map of environment variables to pass to the Lambda function. AWS will automatically encrypt these with KMS and decrypt them when running the function.

<a name="evaluation_periods" className="snap-top"></a>

* [**`evaluation_periods`**](#evaluation_periods) &mdash; The number of periods over which data is compared to the specified threshold.

<a name="file_system_access_point_arn" className="snap-top"></a>

* [**`file_system_access_point_arn`**](#file_system_access_point_arn) &mdash; The ARN of an EFS access point to use to access the file system. Only used if [`mount_to_file_system`](#mount_to_file_system) is true.

<a name="file_system_mount_path" className="snap-top"></a>

* [**`file_system_mount_path`**](#file_system_mount_path) &mdash; The mount path where the lambda can access the file system. This path must begin with /mnt/. Only used if [`mount_to_file_system`](#mount_to_file_system) is true.

<a name="handler" className="snap-top"></a>

* [**`handler`**](#handler) &mdash; The function entrypoint in your code. This is typically the name of a function or method in your code that AWS will execute when this Lambda function is triggered.

<a name="image_uri" className="snap-top"></a>

* [**`image_uri`**](#image_uri) &mdash; The ECR image URI containing the function's deployment package. Example: [`01234501234501.dkr.ecr.us-east-1.amazonaws.com/image_name:image_tag`](#01234501234501.dkr.ecr.us-east-1.amazonaws.com/image_name:image_tag)

<a name="kms_key_arn" className="snap-top"></a>

* [**`kms_key_arn`**](#kms_key_arn) &mdash; A custom KMS key to use to encrypt and decrypt Lambda function environment variables. Leave it blank to use the default KMS key provided in your AWS account.

<a name="lambda_role_permissions_boundary_arn" className="snap-top"></a>

* [**`lambda_role_permissions_boundary_arn`**](#lambda_role_permissions_boundary_arn) &mdash; The ARN of the policy that is used to set the permissions boundary for the IAM role for the lambda

<a name="layers" className="snap-top"></a>

* [**`layers`**](#layers) &mdash; The list of Lambda Layer Version ARNs to attach to your Lambda Function. You can have a maximum of 5 Layers attached to each function.

<a name="memory_size" className="snap-top"></a>

* [**`memory_size`**](#memory_size) &mdash; The maximum amount of memory, in MB, your Lambda function will be able to use at runtime. Can be set in 64MB increments from 128MB up to 1536MB. Note that the amount of CPU power given to a Lambda function is proportional to the amount of memory you request, so a Lambda function with 256MB of memory has twice as much CPU power as one with 128MB.

<a name="metric_name" className="snap-top"></a>

* [**`metric_name`**](#metric_name) &mdash; The name for the alarm's associated metric.

<a name="mount_to_file_system" className="snap-top"></a>

* [**`mount_to_file_system`**](#mount_to_file_system) &mdash; Set to true to mount your Lambda function on an EFS. Note that the lambda must also be deployed inside a VPC [`(run_in_vpc`](#(run_in_vpc) must be set to true) for this config to have any effect.

<a name="name" className="snap-top"></a>

* [**`name`**](#name) &mdash; The name of the Lambda function. Used to namespace all resources created by this module.

<a name="namespace" className="snap-top"></a>

* [**`namespace`**](#namespace) &mdash; The namespace to use for all resources created by this module. If not set, [`lambda_function_name`](#lambda_function_name), with '-scheduled' as a suffix, is used.

<a name="period" className="snap-top"></a>

* [**`period`**](#period) &mdash; The period in seconds over which the specified `statistic` is applied.

<a name="reserved_concurrent_executions" className="snap-top"></a>

* [**`reserved_concurrent_executions`**](#reserved_concurrent_executions) &mdash; The amount of reserved concurrent executions for this lambda function or -1 if unreserved.

<a name="run_in_vpc" className="snap-top"></a>

* [**`run_in_vpc`**](#run_in_vpc) &mdash; Set to true to give your Lambda function access to resources within a VPC.

<a name="runtime" className="snap-top"></a>

* [**`runtime`**](#runtime) &mdash; The runtime environment for the Lambda function (e.g. nodejs, python2.7, java8). See [`https://docs.aws.amazon.com/lambda/latest/dg/API_CreateFunction`](#https://docs.aws.amazon.com/lambda/latest/dg/API_CreateFunction).html#SSS-CreateFunction-request-Runtime for all possible values.

<a name="s3_bucket" className="snap-top"></a>

* [**`s3_bucket`**](#s3_bucket) &mdash; An S3 bucket location containing the function's deployment package. Exactly one of [`source_path`](#source_path) or the [`s3_xxx`](#s3_xxx) variables must be specified.

<a name="s3_key" className="snap-top"></a>

* [**`s3_key`**](#s3_key) &mdash; The path within [`s3_bucket`](#s3_bucket) where the deployment package is located. Exactly one of [`source_path`](#source_path) or the [`s3_xxx`](#s3_xxx) variables must be specified.

<a name="s3_object_version" className="snap-top"></a>

* [**`s3_object_version`**](#s3_object_version) &mdash; The version of the path in [`s3_key`](#s3_key) to use as the deployment package. Exactly one of [`source_path`](#source_path) or the [`s3_xxx`](#s3_xxx) variables must be specified.

<a name="schedule_expression" className="snap-top"></a>

* [**`schedule_expression`**](#schedule_expression) &mdash; An expression that defines the schedule for this lambda job. For example, cron(0 20 * * ? *) or rate(5 minutes). For more information visit https://docs.aws.amazon.com/lambda/latest/dg/services-cloudwatchevents-expressions.html

<a name="should_create_outbound_rule" className="snap-top"></a>

* [**`should_create_outbound_rule`**](#should_create_outbound_rule) &mdash; If true, create an egress rule allowing all outbound traffic from Lambda function to the entire Internet (e.g. 0.0.0.0/0).

<a name="skip_zip" className="snap-top"></a>

* [**`skip_zip`**](#skip_zip) &mdash; Set to true to skip zip archive creation and assume that [`source_path`](#source_path) points to a pregenerated zip archive.

<a name="source_path" className="snap-top"></a>

* [**`source_path`**](#source_path) &mdash; The path to the directory that contains your Lambda function source code. This code will be zipped up and uploaded to Lambda as your deployment package. If [`skip_zip`](#skip_zip) is set to true, then this is assumed to be the path to an already-zipped file, and it will be uploaded directly to Lambda as a deployment package. Exactly one of [`source_path`](#source_path) or the [`s3_xxx`](#s3_xxx) variables must be specified.

<a name="statistic" className="snap-top"></a>

* [**`statistic`**](#statistic) &mdash; The statistic to apply to the alarm's associated metric.

<a name="subnet_ids" className="snap-top"></a>

* [**`subnet_ids`**](#subnet_ids) &mdash; A list of subnet IDs the Lambda function should be able to access within your VPC. Only used if [`run_in_vpc`](#run_in_vpc) is true.

<a name="tags" className="snap-top"></a>

* [**`tags`**](#tags) &mdash; A map of tags to apply to the Lambda function.

<a name="threshold" className="snap-top"></a>

* [**`threshold`**](#threshold) &mdash; The value against which the specified statistic is compared. This parameter is required for alarms based on static thresholds, but should not be used for alarms based on anomaly detection models.

<a name="timeout" className="snap-top"></a>

* [**`timeout`**](#timeout) &mdash; The maximum amount of time, in seconds, your Lambda function will be allowed to run. Must be between 1 and 900 seconds.

<a name="vpc_id" className="snap-top"></a>

* [**`vpc_id`**](#vpc_id) &mdash; The ID of the VPC the Lambda function should be able to access. Only used if [`run_in_vpc`](#run_in_vpc) is true.

<a name="working_directory" className="snap-top"></a>

* [**`working_directory`**](#working_directory) &mdash; The working directory for the docker image. Only used if you specify a Docker image via [`image_uri`](#image_uri).

<a name="zip_output_path" className="snap-top"></a>

* [**`zip_output_path`**](#zip_output_path) &mdash; The path to store the output zip file of your source code. If empty, defaults to module path. This should be the full path to the zip file, not a directory.

</TabItem>
<TabItem value="outputs" label="Outputs">

<a name="alarm_actions" className="snap-top"></a>

* [**`alarm_actions`**](#alarm_actions) &mdash; The list of actions to execute when this alarm transitions into an ALARM state from any other state

<a name="alarm_arn" className="snap-top"></a>

* [**`alarm_arn`**](#alarm_arn) &mdash; ARN of the Cloudwatch alarm

<a name="alarm_name" className="snap-top"></a>

* [**`alarm_name`**](#alarm_name) &mdash; Name of the Cloudwatch alarm

<a name="event_rule_arn" className="snap-top"></a>

* [**`event_rule_arn`**](#event_rule_arn) &mdash; Cloudwatch Event Rule Arn

<a name="event_rule_schedule" className="snap-top"></a>

* [**`event_rule_schedule`**](#event_rule_schedule) &mdash; Cloudwatch Event Rule schedule expression

<a name="function_arn" className="snap-top"></a>

* [**`function_arn`**](#function_arn) &mdash; Amazon Resource Name (ARN) identifying the Lambda Function

<a name="function_name" className="snap-top"></a>

* [**`function_name`**](#function_name) &mdash; Unique name for Lambda Function

<a name="iam_role_arn" className="snap-top"></a>

* [**`iam_role_arn`**](#iam_role_arn) &mdash; Amazon Resource Name (ARN) of the AWS IAM Role created for the Lambda Function

<a name="iam_role_id" className="snap-top"></a>

* [**`iam_role_id`**](#iam_role_id) &mdash; Name of the AWS IAM Role created for the Lambda Function

<a name="insufficient_data_actions" className="snap-top"></a>

* [**`insufficient_data_actions`**](#insufficient_data_actions) &mdash; The list of actions to execute when this alarm transitions into an [`INSUFFICIENT_DATA`](#INSUFFICIENT_DATA) state from any other state

<a name="invoke_arn" className="snap-top"></a>

* [**`invoke_arn`**](#invoke_arn) &mdash; Amazon Resource Name (ARN) to be used for invoking the Lambda Function

<a name="ok_actions" className="snap-top"></a>

* [**`ok_actions`**](#ok_actions) &mdash; The list of actions to execute when this alarm transitions into an OK state from any other state

<a name="qualified_arn" className="snap-top"></a>

* [**`qualified_arn`**](#qualified_arn) &mdash; Amazon Resource Name (ARN) identifying your Lambda Function version

<a name="security_group_id" className="snap-top"></a>

* [**`security_group_id`**](#security_group_id) &mdash; Security Group ID of the Security Group created for the Lambda Function

<a name="version" className="snap-top"></a>

* [**`version`**](#version) &mdash; Latest published version of your Lambda Function

</TabItem>
</Tabs>


<!-- ##DOCS-SOURCER-START
{"sourcePlugin":"service-catalog-api","hash":"e6b48c957b265b26c0407a4f8024f1f2"}
##DOCS-SOURCER-END -->
