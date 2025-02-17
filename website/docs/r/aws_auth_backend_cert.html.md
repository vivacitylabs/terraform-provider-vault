---
layout: "vault"
page_title: "Vault: vault_aws_auth_backend_cert resource"
sidebar_current: "docs-vault-resource-aws-auth-backend-cert"
description: |-
  Manages a certificate for an AWS Auth Backend in Vault.
---

# vault\_aws\_auth\_backend\_cert

Manages a certificate to be used with an AWS Auth Backend in Vault.

This resource sets the AWS public key and the type of document that can be
verified against the key that Vault can then use to verify the instance
identity documents making auth requests.

For more information, see the [Vault
docs](https://www.vaultproject.io/api-docs/auth/aws#configure-client).

~> **Important** All data provided in the resource configuration will be
written in cleartext to state and plan files generated by Terraform, and will
appear in the console output when Terraform runs. Protect these artifacts
accordingly. See [the main provider documentation](../index.html) for more
details.

## Example Usage

```hcl
resource "vault_auth_backend" "aws" {
  type = "aws"
}

resource "vault_aws_auth_backend_cert" "cert" {
  backend         = vault_auth_backend.aws.path}"
  cert_name       = "my-cert"
  aws_public_cert = file("${path.module}/aws_public_key.crt)"
  type            = "pkcs7"
}
```

## Argument Reference

The following arguments are supported:

* `namespace` - (Optional) The namespace to provision the resource in.
  The value should not contain leading or trailing forward slashes.
  The `namespace` is always relative to the provider's configured [namespace](../index.html#namespace).
   *Available only for Vault Enterprise*.

* `cert_name` - (Required) The name of the certificate.

* `aws_public_cert` - (Required) The  Base64 encoded AWS Public key required to
  verify PKCS7 signature of the EC2 instance metadata. You can find this key in
  the [AWS
  documentation](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-identity-documents.html).

* `type` - (Optional) Either "pkcs7" or "identity", indicating the type of
  document which can be verified using the given certificate. Defaults to
  "pkcs7".

* `backend` - (Optional) The path the AWS auth backend being configured was
  mounted at.  Defaults to `aws`.

## Attributes Reference

No additional attributes are exported by this resource.

## Import

AWS auth backend certificates can be imported using `auth/`, the `backend` path, `/config/certificate/`, and the `cert_name` e.g.

```
$ terraform import vault_aws_auth_backend_cert.example auth/aws/config/certificate/my-cert
```
