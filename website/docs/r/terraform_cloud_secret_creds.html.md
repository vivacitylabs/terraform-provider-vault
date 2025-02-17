---
layout: "vault"
page_title: "Vault: vault_terraform_cloud_secret_creds resource"
sidebar_current: "docs-vault-resource-terraform-cloud-secret-creds"
description: |-
  Generates tokens for Terraform Cloud.
---

# vault\_terraform\_cloud\_secret\_creds

Generates tokens for Terraform Cloud.

~> **Important** All data retrieved from Vault will be
written in cleartext to state file generated by Terraform, will appear in
the console output when Terraform runs, and may be included in plan files
if secrets are interpolated into any resource attributes.
Protect these artifacts accordingly. See
[the main provider documentation](../index.html)
for more details.

## Example Usage

```hcl
resource "vault_terraform_cloud_secret_backend" "test" {
  backend     = "terraform"
  description = "Manages the Terraform Cloud backend"
  token       = "V0idfhi2iksSDU234ucdbi2nidsi..."

}

resource "vault_terraform_cloud_secret_role" "example" {
  backend      = vault_terraform_cloud_secret_backend.test.backend
  name         = "test-role"
  organization = "example-organization-name"
  team_id      = "team-ieF4isC..."
}

resource "vault_terraform_cloud_secret_creds" "token" {
  backend = vault_terraform_cloud_secret_backend.test.backend
  role    = vault_terraform_cloud_secret_role.example.name
}
```

## Argument Reference

The following arguments are supported:

* `namespace` - (Optional) The namespace to provision the resource in.
  The value should not contain leading or trailing forward slashes.
  The `namespace` is always relative to the provider's configured [namespace](../index.html#namespace).
   *Available only for Vault Enterprise*.

* `backend` - (Required) The path to the Terraform Cloud secret backend to
read credentials from, with no leading or trailing `/`s.

* `role` - (Required) The name of the Terraform Cloud secret backend role to generate
a token for, with no leading or trailing `/`s.

## Attributes Reference

In addition to the arguments above, the following attributes are exported:

* `token_id` - The public identifier for a specific token. It can be used 
to look up information about a token or to revoke a token.
  
* `token` - The actual token that was generated and can be used with API calls
to identify the user of the call.
  
* `organization` - The organization associated with the token provided.

* `team_id` - The team id associated with the token provided.

* `lease_id` - The lease associated with the token. Only user tokens will have a 
Vault lease associated with them.