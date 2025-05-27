# Acquifier Terraform Backend Provisioning

This project provides a CloudFormation configuration and GitHub Actions workflow for provisioning the necessary resources for a Terraform backend on AWS, enabling access to other Cloud Providers. It uses AWS Systems Manager (SSM) for secure instance management, eliminating the need for SSH access.

## Architecture

The architecture is based on the following components:
- S3 bucket to store the Terraform state.
- IAM role to be assumed by the EC2 instance, allowing it to provision resources using Terraform
- EC2 Instance on which Terraform is installed.

## Prerequisites

*   An AWS account.
*   Configured AWS credentials for GitHub Actions (stored as encrypted secrets).

## Getting Started

1.  **Clone the repository:**

    ```bash
    git clone <git@github.com:ro-nald/aquifer.git>
    cd <aquifer>
    ```

2.  **Configure AWS Credentials:**

    *   Store your AWS access key ID and secret access key as encrypted secrets in your GitHub repository's settings.
    *   Go to your repository on GitHub.
    *   Click on "Settings" -> "Secrets and variables" -> "Actions".
    *   Create two secrets:
        *   `AWS_ACCESS_KEY_ID`: Your AWS access key ID.
        *   `AWS_SECRET_ACCESS_KEY`: Your AWS secret access key.
    *   **Important:** Use an IAM user with limited permissions specifically for deploying CloudFormation stacks. Do *not* use your root account credentials.

3.  **Configuration Files:**

    *   The project uses environment-specific configuration files stored in the `config` directory.
    *   Create the following files:
        *   `config/dev.yml`
        *   `config/staging.yml`
        *   `config/prod.yml`
    *   Modify the configurations to match the specific requirements. An example configuration would look like this:
    ```yaml
    environment: dev
    instance_type: t2.micro
    region: eu-west-2
    ```

4.  **GitHub Actions Workflow:**

    *   The project uses a GitHub Actions workflow defined in `.github/workflows/deploy.yml` to automate the deployment process.
    *   The workflow will:
        *   Deploy the CloudFormation stack.
        *   Execute commands on the EC2 instance using SSM.

5.  **Deploy:**

    *   Push the changes to the main branch or manually trigger the workflow to start the deployment process.

## Configuration

The following files are used to configure the project:

*   `.github/workflows/deploy.yml`: Defines the GitHub Actions workflow for deploying the CloudFormation stack and executing commands on the EC2 instance.
*   `cloudformation/base-infrastructure.yaml`: Defines the CloudFormation template for creating the base infrastructure resources, including the VPC, subnets, security groups, EC2 instance, and S3 bucket.
*   `config/dev.yml`, `config/staging.yml`, `config/prod.yml`: Environment-specific configuration files that define settings such as the AWS region and instance type.

## Security

*   This project uses AWS Systems Manager (SSM) to securely manage the EC2 instance, eliminating the need for SSH access.
*   AWS credentials are stored as encrypted secrets in GitHub Actions.
*   The IAM role for the EC2 instance has limited permissions, granting it only the necessary access to provision resources and interact with SSM.

## SSM (Systems Manager)
SSM(Systems Manager) is used in this project to allow command execution on the created EC2 instance.
Session Manager, a feature of AWS Systems Manager, lets you manage your Amazon EC2 instances through a browser-based shell or through the AWS CLI. Session Manager provides secure and auditable instance management without the need to open inbound ports, maintain bastion hosts, or manage SSH keys.

## Contributing

Contributions are welcome! Please submit a pull request with your changes.

## License

This project is licensed under the MIT License.
