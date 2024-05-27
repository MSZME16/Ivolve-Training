# Lab 19: AWS Security

## Objective
1. Create AWS account.
2. Set up a billing alarm.
3. Create two IAM groups: `admin` and `developer`.
   - `admin` group: Full admin permissions.
   - `developer` group: Access to S3 only.
4. Create users with specific permissions:
   - `admin-1`: Console access only with MFA.
   - `admin-2-prog`: Programmatic access only.
   - `dev-user`: Programmatic and console access.
5. List all users and groups using AWS CLI commands.
6. Verify access for `dev-user` by accessing EC2 and S3, and take screenshots.

## Prerequisites
- An AWS account.
- AWS CLI installed and configured on your local machine.

## Steps

### 1. Create AWS Account
Follow the instructions on the [AWS website](https://aws.amazon.com/) to create an AWS account.

### 2. Set Up a Billing Alarm
1. Go to the CloudWatch dashboard in the AWS Management Console.
2. Select "Alarms" from the left-hand menu.
3. Click "Create alarm".
4. Select "Billing" as the metric.
5. Choose the "EstimatedCharges" metric.
6. Set the threshold (e.g., $10.00).
7. Configure notifications (e.g., email).

### 3. Create IAM Groups

#### Create `admin` Group
1. Navigate to the IAM dashboard.
2. Click "User groups" > "Create group".
3. Name the group `admin`.
4. Attach the `AdministratorAccess` policy.
5. Click "Create group".

#### Create `developer` Group
1. Navigate to the IAM dashboard.
2. Click "User groups" > "Create group".
3. Name the group `developer`.
4. Attach the `AmazonS3FullAccess` policy.
5. Click "Create group".

### 4. Create Users with Specific Permissions

#### Create `admin-1` User
1. Navigate to the IAM dashboard.
2. Click "Users" > "Add user".
3. Enter username `admin-1`.
4. Select "AWS Management Console access".
5. Set console password.
6. Add user to the `admin` group.
7. Click "Next: Tags" and then "Next: Review".
8. Click "Create user".
9. Set up MFA:
   - Go to "Users" > select `admin-1` > "Security credentials" > "Manage" under "MFA device".
   - Follow the instructions to enable MFA.

#### Create `admin-2-prog` User
1. Navigate to the IAM dashboard.
2. Click "Users" > "Add user".
3. Enter username `admin-2-prog`.
4. Select "Programmatic access".
5. Add user to the `admin` group.
6. Click "Next: Tags" and then "Next: Review".
7. Click "Create user".
8. Save the Access Key ID and Secret Access Key.

#### Create `dev-user`
1. Navigate to the IAM dashboard.
2. Click "Users" > "Add user".
3. Enter username `dev-user`.
4. Select both "Programmatic access" and "AWS Management Console access".
5. Set console password.
6. Add user to the `developer` group.
7. Click "Next: Tags" and then "Next: Review".
8. Click "Create user".
9. Save the Access Key ID and Secret Access Key.

### 5. List All Users and Groups Using AWS CLI Commands

1. Open Terminal or Command Prompt.
2. Configure AWS CLI with the credentials of a user that has appropriate permissions:
   ```sh
   aws configure
3. Enter the required details:
   . AWS Access Key ID
   . AWS Secret Access Key
   . Default region name (e.g., us-east-1)
   . Default output format (e.g., json)
4. List all users:
      **aws iam list-users**

5. List all groups:
    **aws iam list-groups**
   
Conclusion
By following these steps, you will successfully create an AWS account, set up billing alarms, create IAM groups and users with specified permissions, list the users and groups using AWS CLI, and verify access for the developer user.

Troubleshooting
Ensure the AWS CLI is correctly configured with valid credentials.
Check permissions for each user to ensure they have the correct policies attached.
Verify the credentials (Access Key ID and Secret Access Key) are correct and active.

References
AWS IAM Documentation -> https://docs.aws.amazon.com/IAM/latest/UserGuide/
AWS CLI Documentation -> https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html
