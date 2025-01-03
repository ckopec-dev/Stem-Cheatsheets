# Azure Cheatsheet

## Terminology

- Resource: a purchased service (e.g. VM, database)
- Resource Group: logical groupings of related resources that share life-cycle, permissions, and policies
- Azure Resource Manager (ARM): centralized management layer for resources and resource groups

### Core services

- Compute (e.g. VMs, containers, k8)
- Storage (e.g. files, blobs, Data Lake)
- Networking (e.g. CDN, vnet manager, vpn gateway)
- Databases (e.g. Cosmo, SQL db, SQL managed instance)

## CLI Installation

### On Windows and verify

- [64 bit version](https://aka.ms/installazurecliwindowsx64)
- C:\az --version

### On Ubuntu
`$ curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash`

### Update the CLI
`$ az upgrade`

### Run in a Docker container
`$ docker run -it mcr.microsoft.com/azure-cli:cbl-mariner2.0`

## Basic usage

### Log in
~~~
# This will spawn a browser to complete the login process.
$ az login

# Set active subscription
$ az account set --subscription {name}

# Show access token
$ az account get-access-token
~~~

## Get list of subscriptions
~~~
# Json format
% az account list
# Table format
$ az account list --output table
~~~

### Find example commands and get help
~~~
# Find examples
$ az find vnet

# Get help
% az account list --help
~~~

