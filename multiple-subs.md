Alright — here’s a **ready-to-use Terraform setup** for deploying resources to **two different subscriptions** (even if they’re in **two different tenants**), without hardcoding secrets.

---

## **Directory Structure**

```
two-subscriptions/
├── main.tf
├── variables.tf
└── terraform.tfvars   # optional
```

---

## **main.tf**

```hcl
terraform {
  required_version = ">= 1.0.0"

  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0"
    }
  }
}

# Provider for Subscription 1
provider "azurerm" {
  alias           = "sub1"
  subscription_id = var.sub1_subscription_id
  client_id       = var.sub1_client_id
  client_secret   = var.sub1_client_secret
  tenant_id       = var.sub1_tenant_id
  features {}
}

# Provider for Subscription 2
provider "azurerm" {
  alias           = "sub2"
  subscription_id = var.sub2_subscription_id
  client_id       = var.sub2_client_id
  client_secret   = var.sub2_client_secret
  tenant_id       = var.sub2_tenant_id
  features {}
}

# Resource Group in Subscription 1
resource "azurerm_resource_group" "rg_sub1" {
  provider = azurerm.sub1
  name     = "rg-sub1"
  location = "East US"
}

# Resource Group in Subscription 2
resource "azurerm_resource_group" "rg_sub2" {
  provider = azurerm.sub2
  name     = "rg-sub2"
  location = "West Europe"
}
```

---

## **variables.tf**

```hcl
variable "sub1_subscription_id" {}
variable "sub1_client_id" {}
variable "sub1_client_secret" {}
variable "sub1_tenant_id" {}

variable "sub2_subscription_id" {}
variable "sub2_client_id" {}
variable "sub2_client_secret" {}
variable "sub2_tenant_id" {}
```

---

## **Environment Variable Setup (Windows PowerShell)**

```powershell
# Subscription 1
$env:TF_VAR_sub1_subscription_id="SUB_ID_1"
$env:TF_VAR_sub1_client_id="APP_ID_1"
$env:TF_VAR_sub1_client_secret="SECRET_1"
$env:TF_VAR_sub1_tenant_id="TENANT_ID_1"

# Subscription 2
$env:TF_VAR_sub2_subscription_id="SUB_ID_2"
$env:TF_VAR_sub2_client_id="APP_ID_2"
$env:TF_VAR_sub2_client_secret="SECRET_2"
$env:TF_VAR_sub2_tenant_id="TENANT_ID_2"
```

---

## **Deploy**

```powershell
terraform init
terraform plan
terraform apply
```

---

✅ **Why this works**

* Each subscription/tenant has its **own aliased provider** (`azurerm.sub1`, `azurerm.sub2`).
* No secrets are in `.tf` files — they come from **environment variables**.
* You can deploy **both subscriptions in a single `terraform apply`**.

---
