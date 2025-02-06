# Reusable GitHub Workflows

This project contains reusable workflows that Asperitas Consulting uses internally. We've elected to make these open source. Feel free to use them.

> Reusable Workflows

| Workflow Name | Workflow File | Description |
| --- | --- | --- |
| Terraform Plan/Apply | ```terraform.yml``` | Will install Terraform and perform a plan, apply, or destroy. |

## Terraform Plan/Apply

### Parameters

| Parameter | Required | Default | Description |
| --- | --- | --- | --- |
| terraform-folder | Yes | - | Directory containing Terraform configuration |
| environment | Yes | - | Environment to deploy (e.g., dev, staging, prod) |
| terraform-version | No | 'latest' | Terraform version to use |
| apply | No | false | Run terraform apply |
| destroy | No | false | Run terraform destroy |

### Required Secrets
The workflow expects the following secrets to be available, depending on your cloud provider:

**Azure:**
- ARM_CLIENT_ID
- ARM_CLIENT_SECRET
- ARM_SUBSCRIPTION_ID
- ARM_TENANT_ID

**AWS:**
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY

Example usage:
```
name: 'IaC-Azure - Virtual Machine Setup'

on:
  workflow_dispatch:
    inputs:
      apply:
        description: 'Apply the Terraform changes'
        type: boolean
        default: false
        required: true
      destroy:
        description: 'Destroy the infrastructure'
        type: boolean
        default: false
        required: true

jobs:
  terraform:
    uses: AsperitasConsulting/reusable-workflows/.github/workflows/terraform.yml@v0.0.1
    with:
      terraform-folder: infrastructure/azure/vm_setup
      environment: demo
      apply: ${{ inputs.apply }}
      destroy: ${{ inputs.destroy }}
    secrets: inherit
