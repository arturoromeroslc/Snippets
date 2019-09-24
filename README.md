# Snippets

```
{
	// Place your snippets for typescript here. Each snippet is defined under a snippet name and has a prefix, body and 
	// description. The prefix is what is used to trigger the snippet and the body will be expanded and inserted. Possible variables are:
	// $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. Placeholders with the 
	// same ids are connected.
	// Example:
	"cdk deps": {
		"prefix": "cdk-deps",
		"body": [
			"import events = require('@aws-cdk/aws-events')",
			"import cloudwatch = require('@aws-cdk/aws-cloudwatch')",
			"import actions = require('@aws-cdk/aws-cloudwatch-actions')",
			"import sns = require('@aws-cdk/aws-sns')",
			"import subs = require('@aws-cdk/aws-sns-subscriptions')",
			"import iam = require('@aws-cdk/aws-iam')",
			"import targets = require('@aws-cdk/aws-events-targets')",
			"import lambda = require('@aws-cdk/aws-lambda')",
			"import cdk = require('@aws-cdk/core')",
			"import path = require('path')",
			"const METRIC_NAME_SPACE = 'e2e'",
			"const DURATION = 2"
		],
		"description": "Log output to console"
	},
	"cdk stack": {
		"prefix": "cdk-stack",
		"body": [
			"export class LambdaE2EStack extends cdk.Stack {",
			"\tconstructor(app: cdk.App, id: string) {",
			"\t\tsuper(app, id)$1",
			"\t}",
			"}"
		],
		"description": "Log output to console"
	},
	"cdk run": {
		"prefix": "cdk-run",
		"body": [
			"const app = new cdk.App()",
			"new LambdaE2EStack(app, 'LambdaE2EExample')",
			"app.synth()"
		],
		"description": "Log output to console"
	},
	"cdk policy": {
		"prefix": "cdk-policy",
		"body": [
			"\t/**",
			"\t * Add permissions for the lambda to be able to log metrics into Cloudwatch",
			"\t */",
			"\tconst statement = new iam.PolicyStatement()",
			"\tstatement.addActions('cloudwatch:PutMetricData')",
			"\tstatement.addResources('*')"
		],
		"description": "Log output to console"
	},
	"cdk function": {
		"prefix": "cdk-function",
		"body": [
			"\t/**",
			"\t * Setup Lambda Function settings",
			"\t */",
			"\tconst lambdaFn = new lambda.Function(this, 'LambdaFunction', {",
			"\t  runtime: lambda.Runtime.NODEJS_8_10,",
			"\t  handler: 'index.handler',",
			"\t  code: lambda.Code.fromAsset(",
			"\t\tpath.join(__dirname, 'lambda-handler/index.zip')",
			"\t  ),",
			"\t  memorySize: 512, //memory size needs to be at least 512, this lambda runs at an average of 480",
			"\t  timeout: cdk.Duration.minutes(5),",
			"\t  tracing: lambda.Tracing.ACTIVE //x-ray tracing, useful for debugging.",
			"\t})"
		],
		"description": "Log output to console"
	},
	"cdk add role": {
		"prefix": "cdk-add-role",
		"body": [
			"\t/**",
			"\t * Add cloudwatch policy statement to lambda",
			"\t */",
			"\tlambdaFn.addToRolePolicy(statement)"
		],
		"description": "Log output to console"
	},
	"cdk rule": {
		"prefix": "cdk-rule",
		"body": [
			"\t/**",
			"\t * Create Cloudwatch rule to run every DURATION minutes on the lambda created above",
			"\t */",
			"\tconst rule = new events.Rule(this, `Run every ${DURATION} minutes`, {",
			"\t  schedule: events.Schedule.rate(cdk.Duration.minutes(DURATION)),",
			"\t  enabled: true",
			"\t})"
		],
		"description": "Log output to console"
	},
	"cdk target": {
		"prefix": "cdk-target",
		"body": [
			"rule.addTarget(new targets.LambdaFunction(lambdaFn))"
		],
		"description": "Log output to console"
	},
	"cdk metric": {
		"prefix": "cdk-metric",
		"body": [
			"\t/**",
			"\t * Create a metric in Cloudwatch where the lambda with log success of failure workflows",
			"\t */",
			"\tconst failedMetric = new cloudwatch.Metric({",
			"\t  namespace: METRIC_NAME_SPACE,",
			"\t  metricName: 'failed-workflow'",
			"\t})"
		],
		"description": "Log output to console"
	},
	"cdk alarm": {
		"prefix": "cdk-alarm",
		"body": [
			"\t/**",
			"\t * Configure alarm on metric data",
			"\t */",
			"\tconst alarm = new cloudwatch.Alarm(this, 'Alarm', {",
			"\t  alarmDescription: 'e2e critical path failed',",
			"\t  metric: failedMetric,",
			"\t  threshold: 1,",
			"\t  evaluationPeriods: 1,",
			"\t  statistic: 'Sum',",
			"\t  period: cdk.Duration.minutes(DURATION),",
			"\t  treatMissingData: cloudwatch.TreatMissingData.BREACHING",
			"\t})"
		],
		"description": "Log output to console"
	},
	"cdk topic": {
		"prefix": "cdk-topic",
		"body": [
			"\t/**",
			"\t * Add an sns topic to know when we are in an alarm state",
			"\t */",
			"\tconst topic = new sns.Topic(this, 'Topic')",
			"\ttopic.addSubscription(new subs.EmailSubscription('artromero801@gmail.com'))",
			"\talarm.addAlarmAction(new actions.SnsAction(topic))"
		],
		"description": "Log output to console"
	},
}
```
