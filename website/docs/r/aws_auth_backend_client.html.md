---
layout: "vault"
page_title: "Vault: vault_aws_auth_backend_client resource"
sidebar_current: "docs-vault-resource-aws-auth-backend-client"
description: |-
  Configures the client used by an AWS Auth Backend in Vault.
---

# vault\_aws\_auth\_backend\_client

Configures the client used by an AWS Auth Backend in Vault.

This resource sets the access key and secret key that Vault will use
when making API requests on behalf of an AWS Auth Backend. It can also
be used to override the URLs Vault uses when making those API requests.

For more information, see the
[Vault docs](https://www.vaultproject.io/api-docs/auth/aws#configure-client).

~> **Important** All data provided in the resource configuration will be
written in cleartext to state and plan files generated by Terraform, and
will appear in the console output when Terraform runs. Protect these
artifacts accordingly. See
[the main provider documentation](../index.html)
for more details.

## Example Usage

```hcl
resource "vault_auth_backend" "example" {
  type = "aws"
}

resource "vault_aws_auth_backend_client" "example" {
  backend    = vault_auth_backend.example.path
  access_key = "INSERT_AWS_ACCESS_KEY"
  secret_key = "INSERT_AWS_SECRET_KEY"
}
```

## Argument Reference

The following arguments are supported:

* `namespace` - (Optional) The namespace to provision the resource in.
  The value should not contain leading or trailing forward slashes.
  The `namespace` is always relative to the provider's configured [namespace](../index.html#namespace).
   *Available only for Vault Enterprise*.

* `backend` - (Optional) The path the AWS auth backend being configured was
	mounted at.  Defaults to `aws`.

* `access_key` - (Optional) The AWS access key that Vault should use for the
	auth backend.

* `secret_key` - (Optional) The AWS secret key that Vault should use for the
	auth backend.

* `ec2_endpoint` - (Optional) Override the URL Vault uses when making EC2 API
	calls.

* `iam_endpoint` - (Optional) Override the URL Vault uses when making IAM API
	calls.

* `sts_endpoint` - (Optional) Override the URL Vault uses when making STS API
	calls.

* `sts_region` - (Optional) Override the default region when making STS API 
    calls. The `sts_endpoint` argument must be set when using `sts_region`.

* `iam_server_id_header_value` - (Optional) The value to require in the
	`X-Vault-AWS-IAM-Server-ID` header as part of `GetCallerIdentity` requests
	that are used in the IAM auth method.

## Attributes Reference

No additional attributes are exported by this resource.

## Import

AWS auth backend clients can be imported using `auth/`, the `backend` path, and `/config/client` e.g.

```
$ terraform import vault_aws_auth_backend_client.example auth/aws/config/client
```
