# Getting Started with Terraform on AWS: Deploying an EC2 VM

Welcome! This guide is your first step into the world of Terraform, a powerful tool for managing "Infrastructure as Code" (IaC). We'll walk you through deploying a simple EC2 virtual machine (VM) on Amazon Web Services (AWS). This example is perfect for beginners, as it uses services available in the AWS Free Tier.

## Understanding Terraform Concepts

### Providers
Terraform interacts with cloud platforms and other services through plugins called **providers**. Each provider (e.g., AWS, Azure, Google Cloud) is a modular executable responsible for understanding API interactions and exposing resources. This modular design means that the AWS provider can be updated and versioned independently of the Azure provider, allowing for faster feature releases.

### The `main.tf` file
Terraform reads configuration files ending in `.tf` to understand what infrastructure you want to create. By convention, the primary configuration is often placed in a file named `main.tf`. This file is written in HCL (HashiCorp Configuration Language), which is designed to be human-readable.

## Step 1: Create a Secure IAM User for Terraform

To follow the **principle of least privilege**, you should create a dedicated IAM (Identity and Access Management) user for Terraform. This user will have only the permissions it needs to manage your AWS resources. Avoid using your root account or a full administrator account for automation.

1.  **Sign in** to the AWS Management Console.
2.  Navigate to the **IAM** service.
3.  Go to **Users** and click **Create user**.
4.  **User name**: Give it a descriptive name like `terraform-user`.
5.  **Provide user access to the AWS Management Console**: Leave this **unchecked**. This user is for programmatic access only (for Terraform), not for logging into the console.
6.  Click **Next**.
7.  On the **Set permissions** page, select **Attach policies directly**.
8.  **Permissions policies**: For learning purposes, you can start with the `AdministratorAccess` policy.
    > **Note:** In a real-world environment, you would create a custom, more restrictive policy that only grants permissions for the specific services Terraform will manage (e.g., EC2, S3, VPC).
9.  Click **Next**, review the details, and click **Create user**.

### Retrieve Your Credentials
After the user is created, you must retrieve its credentials.

1.  Click on the `terraform-user` you just created.
2.  Go to the **Security credentials** tab.
3.  Scroll down to **Access keys** and click **Create access key**.
4.  Select **Command Line Interface (CLI)** as the use case.
5.  Acknowledge the recommendation and click **Next**.
6.  (Optional) Set a description tag, then click **Create access key**.
7.  **This is the only time you will see the Secret Access Key.** Copy both the **Access key ID** and the **Secret access key** and store them securely in a password manager. You will need them in the next step.

## Step 2: Authenticate Terraform with AWS

The most secure and common way to authenticate Terraform to AWS is by using environment variables. This method keeps your secret keys out of your code, which is crucial, especially when your code is stored in a version control system like Git.

### The Provider Block
Your `main.tf` file needs to know which provider to use and what region to operate in. Add the following block to your `main.tf`:

```terraform
provider "aws" {
  region = "us-east-1"
}
```

### Set Environment Variables
Before you run `terraform plan` or `terraform apply`, you need to set the following environment variables in your terminal. Terraform will automatically detect and use them.

Replace `"YOUR_ACCESS_KEY"` and `"YOUR_SECRET_KEY"` with the actual credentials from the IAM user you created.

**On macOS/Linux:**
```bash
export AWS_ACCESS_KEY_ID="YOUR_ACCESS_KEY"
export AWS_SECRET_ACCESS_KEY="YOUR_SECRET_KEY"
```

**On Windows (Command Prompt):**
```bash
set AWS_ACCESS_KEY_ID="YOUR_ACCESS_KEY"
set AWS_SECRET_ACCESS_KEY="YOUR_SECRET_KEY"
```

**On Windows (PowerShell):**
```powershell
$env:AWS_ACCESS_KEY_ID="YOUR_ACCESS_KEY"
$env:AWS_SECRET_ACCESS_KEY="YOUR_SECRET_KEY"
```

## Next Steps

You are now authenticated and ready to define your infrastructure! The next step is to add a `resource` block to your `main.tf` file to describe the EC2 instance you want to create.