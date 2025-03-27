# LocalStack Cleanup Action

This action provides a standardized way to clean up AWS resources in LocalStack after CI/CD testing.

## Overview

This action handles:

1. Setting up the LocalStack environment with proper configuration
2. Verifying LocalStack is running and ready to accept requests
3. Executing Pulumi destroy to remove all deployed resources
4. Ensuring proper AWS environment variables are set
5. Reporting success or failure of the cleanup operation

## Usage

```yaml
- name: Cleanup LocalStack
  uses: hsixlabs/github-actions/localstack-cleanup@main
  with:
    version: "1.0.0"
    stack_name: "org/project/env"
    pulumi_token: ${{ secrets.PULUMI_ACCESS_TOKEN }}
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `go_version` | Go version to use (if needed) | No | `latest` |
| `version` | Version that was deployed | Yes | |
| `stack_name` | Pulumi stack name to destroy | Yes | |
| `work_dir` | Directory containing the Pulumi project | No | `infrastructure` |
| `setup_command` | Command to run for environment setup | No | `''` |
| `pulumi_token` | Pulumi access token | Yes | |
| `aws_default_region` | AWS region to use | No | `us-east-1` |
| `localstack_endpoint` | LocalStack endpoint URL | No | `http://localhost:4566` |
| `localstack_image_tag` | LocalStack Docker image tag to use | No | `latest` |
| `localstack_debug` | Enable LocalStack debug mode | No | `true` |

## Outputs

| Name | Description |
|------|-------------|
| `destroy_success` | Whether the destroy operation was successful (`true` or `false`) |

## Examples

### Basic Usage

```yaml
- name: Cleanup LocalStack
  uses: hsixlabs/github-actions/localstack-cleanup@main
  with:
    version: ${{ needs.deploy.outputs.version }}
    stack_name: "org/project/env"
    pulumi_token: ${{ secrets.PULUMI_ACCESS_TOKEN }}
```

### Always Run on Workflow Completion

```yaml
cleanup:
  name: "Cleanup LocalStack"
  needs: [deploy_job]
  if: always()  # Run cleanup even if previous steps failed
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v4
        
    - name: Cleanup LocalStack
      uses: hsixlabs/github-actions/localstack-cleanup@main
      with:
        version: ${{ needs.deploy_job.outputs.version }}
        stack_name: "org/project/env"
        pulumi_token: ${{ secrets.PULUMI_ACCESS_TOKEN }}
```

### With Custom Setup Command

```yaml
- name: Cleanup LocalStack
  uses: hsixlabs/github-actions/localstack-cleanup@main
  with:
    version: ${{ needs.deploy.outputs.version }}
    stack_name: "org/project/env"
    setup_command: "npm install && go mod download"
    pulumi_token: ${{ secrets.PULUMI_ACCESS_TOKEN }}
```

## How It Works

1. Sets up the Go environment if needed
2. Writes the version to a file for reference
3. Runs any provided setup command
4. Sets up LocalStack using the official LocalStack GitHub Action
5. Verifies LocalStack is ready to accept requests
6. Destroys resources using Pulumi with the correct AWS configuration for LocalStack
7. Reports success or failure of the cleanup operation

## Troubleshooting

### Common Issues

- **LocalStack not running**: The action will attempt to start LocalStack if it's not already running
- **Destroy operation times out**: May indicate complex resource dependencies; check Pulumi logs
- **Resources not fully removed**: When destroy fails, resources may remain in LocalStack requiring manual cleanup

### Important Notes

- The action uses `continue-on-error: true` for the destroy operation, so the workflow won't fail if cleanup fails
- Success/failure is reported via the `destroy_success` output which can be checked in subsequent steps
- Always use this action in an `if: always()` block to ensure cleanup runs even when previous steps fail
