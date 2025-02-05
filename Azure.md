# Azure Cheatsheet

## Accounts

- One account (tenant) can have multiple subscriptions to separate resources by billing responsibility

## Terminology

- On Premises: managing the full technology stack in your own infrastructure (hardware, software, connectivity, etc)
- Infrastructure as a Service (IaaS): cloud service provider handles servers and networking
- Platform as a Service (PaaS): cloud service provider manages runtime, middleware, and OS (plus IaaS)
- Software as a Service (SaaS): cloud service provider manages everything
- Resource: a purchased service (e.g. VM, database)
- Resource Group: logical groupings of related resources that share life-cycle, permissions, and policies
- Azure Resource Manager (ARM): centralized management layer for resources and resource groups
- Service Principal: identity created for use with automated tools that has limited access to resources

### Services managed by cloud service provider

|On Premises|IAAS|PAAS|SAAS|
|-----------|----|----|----|
||||Applications|
||||Data|
|||Runtime|Runtime|
|||Middleware|Middleware|
|||O/S|O/S|
||Virtualization|Virtualization|Virtualization|
||Servers|Servers|Servers|
||Storage|Storage|Storage|
||Networking|Networking|Networking|

### Core services

- Compute (e.g. VMs, containers, k8)
- Storage (e.g. files, blobs, Data Lake)
- Networking (e.g. CDN, vnet manager, vpn gateway)
- Databases (e.g. Cosmo, SQL db, SQL managed instance)

## CLI 

### Installation on Windows

- [64 bit version](https://aka.ms/installazurecliwindowsx64)

### Installation on Ubuntu
`$ curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash`

### Check version
`$ az --version`

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

### Log out
`$ az logout`

### Get list of subscriptions
~~~
# Json format
% az account list
# Table format
$ az account list --output table
~~~

### Clear subscription cache
`$ az account clear`

### Find example commands and get help
~~~
# Find examples
$ az find vnet

# Get help
% az account list --help
~~~

### List resource gruops
`$ az group list --output table`

### Delete resource group
`$ az group delete --name {name}`

### List service credentials
~~~
# Formatted as a table
$ az ad sp list --output table

# Use --all if expected result is missing
$ az ad sp list --all --output table
~~~

### Create a service principal
~~~
# Nothing specified
$ az ad sp create-for-rbac

# With a given name, role, and scope
$ az ad sp create-for-rbac  --name myScope \
                            --role reader \
                            --scope /subscriptions/{subscription-id}
~~~

## Virtual Networks

### Allowable ranges

- 10.0.0.0 - 10.255.255.255 (10/8 prefix)
- 172.16.0.0 - 172.31.255.255 (172.16/12 prefix)
- 192.168.0.0 - 192.168.255.255 (192.168/16 prefix)

### Good resource naming convention

~~~
{Resource-type}-{Workload/Application}-{Environment}-{Region}-{Instance}

e.g. rg-sharepoint-uat-westus-001
~~~

## Load Balancing

- Azure Load Balancer: zone redundant, supports all UDP and TCP protocols
- Traffic Manager: dns-based traffic balancer working at the domain level
- Azure Application Gateway: used to optimize web farm productivity
- Azure Front Door: global load balancing and high availability for web apps

## Application Gateways

- Uses round-robin process to load balance requests to pool
- Session stickiness makes sure client requests are routed to the same server each time

### Main components

- Front-end IP address: a single public address and/or a single private address
- Listeners: accept traffic based on a combination of protocol, port, host, and IP
- Routing rules: binds a listener to a backend pool

## Azure Cosmos DB

## Azure SQL Database

- Analagous to a single Sql Server database 

## Azure SQL Managed Instance

- Analagous to a Sql Server

## Azure Database for MySQL

## Azure Database for PostgreSQL

## Azure Synapse Analytics

- This service was formerly known as Azure SQL Data Warehouse
- Combines data warehousing with analytics
