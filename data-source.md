Data source :
---------------------
Use this data source to access information about an existing resouces like keyvault etc

reference : https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/data-sources/key_vault
reference : https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/data-sources/key_vault_secret

```hcl

data "azurerm_key_vault" "example" {
  name                = "mykeyvault"
  resource_group_name = "some-resource-group"
}

output "vault_uri" {
  value = data.azurerm_key_vault.example.vault_uri
}

data "azurerm_key_vault_secret" "example" {
  name         = "secret-sauce"
  key_vault_id = data.azurerm_key_vault.existing.id
}
----------------------------
main.tf
------------------------------
password = "${data.azurerm_key_vault_secret.example.value}"
```



