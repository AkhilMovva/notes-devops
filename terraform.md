# Terraform

## Commands

```bash
terraform init -backend-config "key=$TF_VAR_environment/$TF_VAR_project" 
 
terraform workspace show
terraform workspace list
terraform workspace new dev
terraform workspace select stag
terraform apply -var-file dev-variables.tfvars -auto-approve
```

## Links

VPC with private, public subnets and nat - <https://medium.com/appgambit/terraform-aws-vpc-with-private-public-subnets-with-nat-4094ad2ab331>
