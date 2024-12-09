# Terraform Cheatsheet

- Infrastructure-as-Code (IaC) tool for configuring + deploying cloud infrastructure
- Uses config files to define the desired infrastructure state
- Management is via providers

## Azure providers

- AzureRM: manage stable resources (e.g. VMs)
- AzAPI: manage stable resources via an API
- AzureAD: manage Entra resources (e.g. users, groups)
- AzureDevops: manage DevOps resources (e.g. pipelines)
- AzureStack: manage Stack Hub resources (e.g. DNS, vnets)

## Basic commands

### Initialize terraform with main.tf configuration file in working directory

`$ terraform init`

### Apply a configuration (i.e. deploy the changes)

`$ terraform apply`

### Remove a configuration

`$ terraform destroy`
