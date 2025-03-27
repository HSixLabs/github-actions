# LocalStack Deploy Action

This action provides a standardized way to deploy AWS resources to LocalStack for testing in CI/CD environments.

## Overview

LocalStack deployment in CI/CD can be challenging to set up correctly. This action handles:

1. Setting up the LocalStack environment with proper configuration
2. Verifying LocalStack is running and ready to accept requests
3. Executing Pulumi deployment against LocalStack
4. Ensuring proper AWS environment variables are set

## Usage

```yaml
- name: Deploy to LocalStack
  uses: hsixlabs/github-actions/localstack-deploy@main
  with:
    version: "1.0.0"
    stack_name: "org/project/env"
    pulumi_token: ${{ secrets.PULUMI_ACCESS_TOKEN }}
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `go_version` | Go version to use (if needed) | No | `latest` |
| `version` | Version to deploy | Yes | |
| `stack_name` | Pulumi stack name to deploy | Yes | |
| `work_dir` | Directory containing the Pulumi project | No | `infrastructure` |
| `setup_command` | Command to run for environment setup | No | `''` |
| `version_command` | Command to run for setting the version | No | `''` |
| `build_command` | Command to run for building resources | No | `''` |
| `pulumi_token` | Pulumi access token | Yes | |
| `aws_default_region` | AWS region to use | No | `us-east-1` |
| `localstack_endpoint` | LocalStack endpoint URL | No | `http://localhost:4566` |
| `localstack_image_tag` | LocalStack Docker image tag to use | No | `latest` |
| `localstack_debug` | Enable LocalStack debug mode | No | `true` |

## Outputs

| Name | Description |
|------|-------------|
| `pulumi_output` | The output from the Pulumi deployment |

## Examples

### Basic Usage

```yaml
- name: Deploy to LocalStack
  uses: hsixlabs/github-actions/localstack-deploy@main
  with:
    version: ${{ needs.release.outputs.version }}
    stack_name: "org/project/env"
    pulumi_token: ${{ secrets.PULUMI_ACCESS_TOKEN }}
```

### With Custom Build Steps

```yaml
- name: Deploy to LocalStack
  uses: hsixlabs/github-actions/localstack-deploy@main
  with:
    version: ${{ needs.release.outputs.version }}
    stack_name: "org/project/env"
    setup_command: "npm install && go mod download"
    build_command: "npm run build:lambda"
    pulumi_token: ${{ secrets.PULUMI_ACCESS_TOKEN }}
```

### With Custom LocalStack Configuration

```yaml
- name: Deploy to LocalStack
  uses: hsixlabs/github-actions/localstack-deploy@main
  with:
    version: "1.0.0"
    stack_name: "org/project/env"
    localstack_image_tag: "2.0.0"
    localstack_debug: "false"
    localstack_endpoint: "http://localstack:4566"
    pulumi_token: ${{ secrets.PULUMI_ACCESS_TOKEN }}
```

## How It Works

1. Sets up the Go environment if needed
2. Writes the version to a file for reference
3. Runs any provided setup, version, and build commands
4. Sets up LocalStack using the official LocalStack GitHub Action
5. Verifies LocalStack is ready to accept requests
6. Deploys resources using Pulumi with the correct AWS configuration for LocalStack

## Troubleshooting

### Common Issues

- **LocalStack fails to start**: Ensure you have enough resources allocated to Docker
- **Pulumi fails to connect to LocalStack**: Verify the `localstack_endpoint` is accessible from the runner
- **AWS resources not created**: Ensure your Pulumi code is using the correct environment variables

### Logs

The action outputs detailed logs including:

- LocalStack startup verification
- Resource deployment status
- Any errors encountered during deployment
