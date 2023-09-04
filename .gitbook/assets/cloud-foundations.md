# Cloud foundations

## Introduction

### Topics

- Introduction
- Cloud managemetn
- Cloud services
- Cloud providers

### Learning objectives

After completing this course, you will be able to :

- Define what is meant by "the cloud".
- Explain why organizations are moving workloads to the cloud.
- Identify services that are provided in the cloud.
- List the major cloud providers.

## What is the Cloud ?

### Cloud architecture

**On-premises information systems architecture**

- Workload
- Services
- Virtual machine
- Virtualization : platform, maintenance, licensing
- Physical infrastructure : power, network, racks, storage
- Physical facility : cost of space, physical security, personnel

**Cloud architecture**

This is the same architecture of the above list. The cloud takes care of **Physical facility**, **Physical infrastructure** and **Virtualization**. It's providing data centers.

- Workload
- Services
- Virtual machine
- **Management plane : ** it is managing functionality for the workload.
- Virtualization
- Physical infrastructure
- Physical facility

### Types of Cloud Services

- Software as a service (SaaS).
- Platform as a service (PaaS).
- Infrastructure as a service (IaaS).

### Accessing Cloud Services

- Internet
- VPN
- Private circle service

## Who are the Cloud Providers ?

- Cloud Market
- AWS :  launched 2006, over 1.000.000 active users, over 190 countries, 18 global regions, over 175 products and services.
- Azure : launched 2010, over 169 services, over 300k customers, 57 regions, 31 geographies.
- Google Cloud Platform : launched 2010, over 90 services, not sure how many users.
- Other Cloud Providers
  - In the neighborhood of 20% of the market.
  - Tencent and Alibaba in China.
  - IBM and Oracle.
  - Digital Ocean.
  - Private cloud providers.

![](img/2.png)

## Why choose the Cloud ?

**Cloud economics**

**CapEx vs. OpEx**

- On-premises capacity expansion = capital expense
  - Purchase equipment and licensing up-front
  - Depreciate and replace equipment
  - Renew licenses
- Cloud-base capacity expansion = operational epxense
  - Billed monthly for what is used
  - No equipment purchase
  - May or may not require license purchase
- Capacity reduction
  - On-premises : possibly sell excess equipment
  - Cloud : reduce monthly cost

**Consumption-based spending**

- Capacity-based spending
  - On-premises resources
  - Some cloud resources - virtual machines
- Consumption-based spending
  - Pay only for what is used
  - Functions, lambda
  - Services
  - Storage

**Functional Advantages**

- Provision environments in minutes rather than days, weeks, or months
  - No capital equipment purchases
  - Streamlined provisioning process
- Built-in access and allocation management
- Reduced administrative overhead

**Also, maybe not**

- Existing investment
- On-going operational expenses
- Data fencing
- Regulatory compliance

## Managing Cloud services

### Tools

- Web-based
  - Azure : https://portal.azure.com
  - AWS : https://console.aws.amazon.com
  - GCP : https://console.cloud.google.com
- Command line
  - AWS : [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) & [PowerShell](https://docs.aws.amazon.com/en-US/cli/azure/install-azure-cli)
  - Azure : [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) & PowerShell (`Install-Module -Name Az -AllowClobber -Scope CurrentUser`)
  - Google Cloud : Google Cloud SDK, Component management (kubectl) & [Cloud Tools for PowerShell](https://cloud.google.com/tools/powershell/docs/quickstart)
  - Cloud shell
- REST API

### Demo

```bash
aws ec2 describe-instances     # AWS
gcloud compute instances list  # GCloud
az vm list                     # Azure
```

## Cloud cost management

### Cloud pricing models

### Calculate resource costs

- https://azure.microsoft.com/en-us/pricing/calculator/

### Cloud billing

- Billing entity
- Billing cycle
- Billing management
- Billing rate
- Marketplace billing

### Cost monitoring

- Budgets
- Alerts
- Monitoring tools
- Third party

### Cost optimization

- Agents
  - Azure advisors
  - AWS cost anomaly detection
  - Google recommender
- Sizing
- Autoscale
- "Serverless" options
- Long-term commitments

Demo : optimize storage cost

























