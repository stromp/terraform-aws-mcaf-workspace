# terraform-aws-mcaf-workspace

Terraform module to create a Terraform Cloud workspace and either a IAM user or role in an AWS account. The user or role credentials are added to the workspace so that Terraform can create resources in the AWS account.

## Usage

### Team access

This module supports assigning an existing team access to the created workspace.

To do this, pass a map to `var.team_access` using the team name as the key and either `access` or `permissions` to assign a team access to the workspace.

Example using a pre-existing role (see [this link](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/team_access#access) for allowed values):

```hcl
team_access = {
  "MyTeamName" = {
    access = "write"
  }
}
```

Example using a custom role (see [this link](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/team_access#permissions) for a list of keys and their allowed values):

```hcl
team_access = {
  "MyTeamName" = {
    permissions = {
      run_tasks         = false
      runs              = "apply"
      sentinel_mocks    = "read"
      state_versions    = "read-outputs"
      variables         = "write"
      workspace_locking = true
    }
  }
}
```

The above custom role is similar to the "write" pre-existing role, but blocks access to the workspace state (which is considered sensitive).

<!--- BEGIN_TF_DOCS --->
## Requirements

| Name | Version |
|------|---------|
| terraform | >= 1.2.0 |

## Providers

| Name | Version |
|------|---------|
| random | n/a |
| tfe | n/a |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| name | A name for the Terraform workspace | `string` | n/a | yes |
| oauth\_token\_id | The OAuth token ID of the VCS provider | `string` | n/a | yes |
| tags | A mapping of tags to assign to resource | `map(string)` | n/a | yes |
| terraform\_organization | The Terraform Enterprise organization to create the workspace in | `string` | n/a | yes |
| agent\_pool\_id | Agent pool ID, requires "execution\_mode" to be set to agent | `string` | `null` | no |
| agent\_role\_arn | IAM role ARN used by Terraform Cloud Agent to assume role in the created account | `string` | `null` | no |
| auth\_method | Configures how the workspace authenticates with the AWS account (can be iam\_role or iam\_user) | `string` | `"iam_user"` | no |
| auto\_apply | Whether to automatically apply changes when a Terraform plan is successful | `bool` | `false` | no |
| branch | The git branch to trigger the TFE workspace for | `string` | `"main"` | no |
| clear\_text\_env\_variables | An optional map with clear text environment variables | `map(string)` | `{}` | no |
| clear\_text\_hcl\_variables | An optional map with clear text HCL Terraform variables | `map(string)` | `{}` | no |
| clear\_text\_terraform\_variables | An optional map with clear text Terraform variables | `map(string)` | `{}` | no |
| execution\_mode | Which execution mode to use | `string` | `"remote"` | no |
| file\_triggers\_enabled | Whether to filter runs based on the changed files in a VCS push | `bool` | `true` | no |
| global\_remote\_state | Allow all workspaces in the organization to read the state of this workspace | `bool` | `null` | no |
| path | Path in which to create the iam\_role or iam\_user | `string` | `null` | no |
| permissions\_boundary\_arn | ARN of the policy that is used to set the permissions boundary for the IAM role or IAM user | `string` | `null` | no |
| policy | The policy to attach to the pipeline role or user | `string` | `null` | no |
| policy\_arns | A set of policy ARNs to attach to the pipeline user | `set(string)` | `[]` | no |
| project\_id | ID of the project where the workspace should be created | `string` | `null` | no |
| region | The default region of the account | `string` | `null` | no |
| remote\_state\_consumer\_ids | A set of workspace IDs set as explicit remote state consumers for this workspace | `set(string)` | `null` | no |
| repository\_identifier | The repository identifier to connect the workspace to | `string` | `null` | no |
| role\_name | The IAM role name for a new pipeline user | `string` | `null` | no |
| sensitive\_env\_variables | An optional map with sensitive environment variables | `map(string)` | `{}` | no |
| sensitive\_hcl\_variables | An optional map with sensitive HCL Terraform variables | <pre>map(object({<br>    sensitive = string<br>  }))</pre> | `{}` | no |
| sensitive\_terraform\_variables | An optional map with sensitive Terraform variables | `map(string)` | `{}` | no |
| slack\_notification\_triggers | The triggers to send to Slack | `list(string)` | <pre>[<br>  "run:created",<br>  "run:planning",<br>  "run:needs_attention",<br>  "run:applying",<br>  "run:completed",<br>  "run:errored"<br>]</pre> | no |
| slack\_notification\_url | The Slack Webhook URL to send notification to | `string` | `null` | no |
| ssh\_key\_id | The SSH key ID to assign to the workspace | `string` | `null` | no |
| team\_access | Map of team names and either type of fixed access or custom permissions to assign | <pre>map(object({<br>    access = optional(string, null),<br>    permissions = optional(object({<br>      run_tasks         = bool<br>      runs              = string<br>      sentinel_mocks    = string<br>      state_versions    = string<br>      variables         = string<br>      workspace_locking = bool<br>    }), null)<br>  }))</pre> | `{}` | no |
| terraform\_version | The version of Terraform to use for this workspace | `string` | `"latest"` | no |
| trigger\_prefixes | List of repository-root-relative paths which should be tracked for changes | `list(string)` | <pre>[<br>  "modules"<br>]</pre> | no |
| username | The username for a new pipeline user | `string` | `null` | no |
| working\_directory | A relative path that Terraform will execute within | `string` | `"terraform"` | no |
| workspace\_tags | A list of tag names for this workspace. Note that tags must only contain lowercase letters, numbers, colons, or hyphens | `list(string)` | `null` | no |

## Outputs

| Name | Description |
|------|-------------|
| arn | The workspace IAM user ARN |
| workspace\_id | The Terraform Cloud workspace ID |

<!--- END_TF_DOCS --->
