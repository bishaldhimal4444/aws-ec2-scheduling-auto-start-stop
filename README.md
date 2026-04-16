# aws-ec2-scheduling-auto-start/stop
AWS EC2 Auto Start/Stop.
## 1. Architecture Overview:
EventBridge  ->  Lambda  ->  EC2  (Tag-Based Scheduling)

Amazon EventBridge Scheduler (cron): ```Fires at 8 AM (start) and 8 PM (stop) daily``` \
AWS Lambda Function: ```Receives action payload, queries EC2 by tag, executes start/stop``` \
Amazon EC2: ```Only instances tagged AutoSchedule=true are affected```

## 2. AWS Services Used: 
|Service	| Purpose	| Key Setting |
|----------|----------|-------------|
|Amazon EventBridge|Triggers Lambda on cron schedule|Timezone: Asia/Kolkata|
|AWS Lambda|Runs start/stop logic on EC2|Runtime: Python 3.12, Timeout: 30s|
|Amazon EC2|Target instances controlled by tags|Tag: AutoSchedule=true|
|AWS IAM|Least privilege permissions|Tag-conditioned start/stop policy|
|Amazon CloudWatch|Logs, alarms, error monitoring|30-day retention + error alarm|
|Amazon SNS|Email alerts on Lambda failure|Email subscription confirmed|
|Amazon SQS|Dead Letter Queue (failed events)|Catches events Lambda fails to handle|

## Step-1: Tag your Instances
Tag EC2 instances so Lambda can find them dynamically. Only tagged instances are started or stopped — everything else is untouched.

How to Add the Tag
1.	Go to AWS Console -> EC2 -> Instances \
2.	Select the instance you want on the schedule \
3.	Click Actions -> Instance Settings -> Manage Tags \
4.	Click Add Tag and enter exactly:

|Tag Key|Tag Value|
|AutoSchedule|true|

5.	Click Save — repeat for every instance you want scheduled

## Step-2: 
