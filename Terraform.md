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



### Example configuraton for provisioning a docker-based web server (save as main.tf)

~~~
# This is a single line comment.

terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "tutorial"

  ports {
    internal = 80
    external = 8000
  }
}
~~~

### Initialize terraform with main.tf configuration file in working directory

~~~
# This downloads and installs the specified providers.
$ terraform init
~~~

### Format the configuration for readability

~~~
# Prints out the names of the files modified, if any.
$ terraform fmt
~~~

### Verify the configuration is valid and consistent

`$ terraform validate`

### Compares Terraform state with the actual state of hte infrastructure
`$ terraform plan`

### Create the infrastructure (i.e. deploy the docker container web server)

`$ terraform apply`

### View the infrastructure state

`$ terraform show`

### View the managed resources in your state

`$ terraform state list`

### Delete the managed resources

`$ terraform destroy`


