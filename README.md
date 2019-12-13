# Vault AWS auth engine demo

Original project: [Vault Agent with AWS](https://learn.hashicorp.com/vault/identity-access-management/vault-agent-aws)

---
## 概要

VaultのAWSでのAuthenticationのデモになります。AWS auth methodについては、[こちら](https://www.vaultproject.io/docs/auth/aws.html)を参照ください。

AWS auth methodには２つのタイプがあります。`iam`と`ec2`の２種類です。
`iam`methodでは、IAMクレデンシャルでサインされた特別なAWSリクエストに対して認証を行います。IAMクレデンシャルはIAM instance profileやLambdaなどで自動的に作成されるので、AWS上のほぼ全てのサービスに対して利用できます。

`ec2`methodは、AWSがEC2インスタンスに自動的に付与するメタデータを用いて認証を行います。よって、この認証方法はEC2のインスタンスにしか利用できません。

`ec2`methodは`iam`methodの登場の前に開発されたもので、現在のベスト・プラクティスとしてはより柔軟かつ高度なアクセスコントロールのある`iam`methodを推奨しています。

## Demo setup

1.
まずは、`terraform.tfvars.example`を`terraform.tfvars`と変名して、中身を環境に合わせて変更してください。

```hcl
#-------------------
# Required
#-------------------

# SSH key name to access EC2 instances. This should already exist in the AWS Region
key_name = "MY_EC2_KEY_NAME"

# AWS region & AZs
aws_region = "ap-northeast-1"
availability_zones = "ap-northeast-1a"

#-----------------------------------------------
# Optional: To overwrite the default settings
#-----------------------------------------------

# All resources will be tagged with this (default is 'vault-agent-demo')
environment_name = "vault-agent-demo"

# Instance size (default is t2.micro)
instance_type = "t2.micro"

# Number of Vault servers to provision (default is 1)
vault_server_count = 1
```

2.
Terraformでプロビジョニングします。

```shell
$ terraform init

$ terraform plan

# Output provides the SSH instruction
$ terraform apply -auto-approve
```
