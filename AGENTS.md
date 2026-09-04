# Infrastructure Engineering Guidelines

## Mandatory Workflow

Every time you create, modify, or remove an infrastructure resource or Terraform module, follow this workflow.

### 1. Understand the Change

Before writing code:

- Inspect the existing module structure.
- Read the relevant documentation.
- Check existing naming conventions.
- Check required tags.
- Check security requirements.
- Reuse existing modules when possible.
- Do not create duplicate functionality.


### 2. Update Documentation

After creating or modifying infrastructure, ALWAYS update the relevant documentation.

Documentation must describe:

- What the resource/module does.
- Required variables.
- Important configuration.
- Dependencies.
- Outputs.
- Security considerations.
- Networking requirements.
- Example usage.

If the module has a README, update:

`modules/<module>/README.md`

Do not leave documentation outdated after an infrastructure change.

Example:

If you create:

`modules/storage/main.tf`

you must also check and update:

`modules/storage/README.md`


## Terraform

- Use Terraform >= 1.16.
- Use AzureRM provider.
- Pin provider versions.
- Do not use azurerm resources with deprecated arguments.
- Every resource must have mandatory tags.
- Do not hardcode secrets.
- Prefer managed identities over service principals.
- Each resource should be as separate module
- Each module should be able to create multiple resources
- Modules should be in `modules/<module>` folder structure
- Prefer to use default values for resources
- Modules should be called from main.tf file for stack
- Input variables to main.tf file should be separete files per each environment


## Networking

- Workloads must use private endpoints.
- Public IPs require explicit justification.
- NSGs must follow least privilege.
- Private endpoints should be able to enable by conditio s

## Security
- Follow SOC 2 security requirments
- Enable encryption at rest
- Enforce HTTPS/TLS 1.2 minimum

## Naming

- Resources must follow: <project>-<environment>-<resource>-<region>
- use `locals.tf` inside module to concat the name
- To each resource add tags:
 `environment`
 `region`
 `project`

 ## Module requirments
 - each resource should ignore if tags will be changed outside of Terraform

 ## Testing
- After every Terraform change in a module, run `terraform init -backend=false` in that module directory, then run `terraform validate`.
- Do not consider a module change complete until validation succeeds.
- If the module depends on the root stack, validate from the module directory first and then from the root configuration if needed.
- Record validation status in the change summary when relevant.