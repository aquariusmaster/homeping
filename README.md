# HomePing - Router Connectivity Monitor

HomePing is an AWS Lambda-based solution that monitors the connectivity of a specified router by checking TCP connectivity on a specified port. If the router becomes unreachable, HomePing sends an email alert via Amazon SES to notify you of the connectivity issue.

![image](https://github.com/user-attachments/assets/3f519127-6fd0-42f7-b656-51406ddfd830)

## Features

- Monitors TCP connectivity to a specified router host and port.
- Sends email alerts using Amazon SES if the router becomes unreachable.
- Configurable host and port settings via CloudFormation parameters.
- Logs all checks in AWS CloudWatch with a configurable retention policy.

## Architecture

This solution consists of the following AWS resources:

- **AWS Lambda**: Runs the connectivity checks on a schedule.
- **Amazon SES**: Sends email alerts if the router is unreachable.
- **AWS CloudWatch Logs**: Stores logs of each check for troubleshooting and monitoring.

## Prerequisites

- AWS account with permissions to deploy resources.
- Verified email addresses in Amazon SES for both sender and recipient.
- AWS CLI and AWS CloudFormation setup (if deploying via command line).

## Deployment

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/HomePing.git
cd HomePing
```

### 2. Deploy the Stack with AWS CloudFormation

Make sure to customize the parameters as needed.

```bash
aws cloudformation deploy \
    --template-file template.yaml \
    --stack-name HomePing \
    --parameter-overrides \
        RouterHost="yourrouterhostnameorip.com" \
        RouterPort=8443 \
        SenderEmail="your-sender-email@example.com" \
        RecipientEmail="your-recipient-email@example.com"
```

### 3. Verify SES Email Addresses

Ensure that both `SenderEmail` and `RecipientEmail` are verified in Amazon SES, or that your SES account is in production mode.

## Usage

Once deployed, the Lambda function will automatically run on a schedule (default is every 5 minutes) to check the router connectivity. Logs will be available in AWS CloudWatch Logs under the `/aws/lambda/HomePingFunction` log group.

If the router is unreachable, you will receive an email alert with the details.

## Customization

- **Schedule**: Adjust the check frequency by modifying the `ScheduleExpression` in the CloudFormation template.
- **Log Retention**: Adjust the `RetentionInDays` for CloudWatch logs in the template to keep logs for a different period.

## Troubleshooting

- **Connectivity Failures**: Check the CloudWatch Logs for details if the function fails to connect to the router.
- **Email Not Received**: Ensure both `SenderEmail` and `RecipientEmail` are verified in Amazon SES, or check your SES sending limits.

## License

This project is licensed under the MIT License.
