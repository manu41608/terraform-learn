reference : https://spacelift.io/blog/terraform-workspaces
```hcl
$ terraform workspace --help
Usage: terraform [global options] workspace

  new, list, show, select and delete Terraform workspaces.

Subcommands:
    delete    Delete a workspace
    list      List Workspaces
    new       Create a new workspace
    select    Select a workspace
    show      Show the name of the current workspace

    by defaullt terraform will add default workspace .

```
 to create :
    terraform workspace new prod

    to show:
    terraform workspace show

    to select:
    terraform workspace select prod
