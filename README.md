🇺🇸 [English](README.md) | 🇧🇷 [Português](README.pt-BR.md)

# Lambda-EC2-modify

AWS Lambda + EventBridge to change an EC2 instance's type on a schedule.

The Lambda function stops the instance, waits for it to stop, changes the instance type, and starts it again. The instance region, ID, and target type are provided by the EventBridge rule as a constant JSON input.

## Repository Files

- `EC2 policy.txt`: IAM policy (JSON) for the Lambda role.
- `Lambda Function.txt`: Lambda function code (Node.js).
- `Constant eventbridge.txt`: Constant JSON input sent by the EventBridge rule.

## Step 1: Create the Policy

- Go to: IAM console > Policies > Create policy > select the **JSON** tab > delete all existing content and paste the content of `EC2 policy.txt` from this repository.
- Name the policy and click **Create policy**.

## Step 2: Create the Role

- Go to: IAM console > Create role > select **AWS service** > **Lambda** > attach the policy created in Step 1.
- Name the role and click **Create role**.

## Step 3: Create the Lambda Function

- Go to: Lambda console > Create function > **Author from scratch** > under **Function name**, enter `modify-instance` or something similar > under **Runtime**, select **Node.js 16.x** > expand **Change default execution role**, select **Use an existing role**, choose the role created in Step 2, and click **Create function**.
- Configure the function code: open the Lambda function, delete the content of the `index.js` tab, and paste the content of `Lambda Function.txt` from this repository. Click **Deploy**.
- Configure the function: go to **Configuration** > **General configuration** > **Edit** > set the timeout to 10 minutes.

## Step 4: Create the EventBridge Rule

- Go to: EventBridge console > Rules > Create rule.
- Name the rule and, under **Rule type**, select **Schedule**.
- Click **Continue to create rule**, enter the schedule as a cron expression, and click **Next**.
- Add the target: select **AWS service**, search for **Lambda function**, and select the function created in Step 3.
- Expand **Additional settings**, select **Constant (JSON text)**, and paste the content of `Constant eventbridge.txt` from this repository (filled in with your instance's values).
- Under **Retry attempts**, enter 40, then click **Next** twice.
- Click **Create rule**.

After these steps, the Lambda + EventBridge setup is configured and ready to use. To test the Lambda function, open it and go to **Test** > **Configure test event**, then paste the content of your EventBridge constant into **Event JSON**.
