## Mise en pratique

----

### Installation

```bash
$ curl -O https://releases.hashicorp.com/terraform/0.13.3/terraform_0.13.3_linux_amd64.zip
$ sudo unzip terraform_0.13.3_linux_amd64.zip /usr/local/bin
$ rm terraform_0.13.3_linux_amd64.zip
```

```bash
$ terraform --version
 Terraform v0.13.3
```

----

### Initialisation

```bash
$ terraform init

Initializing the backend...

Initializing provider plugins...
- Finding latest version of hashicorp/aws...
- Installing hashicorp/aws v3.8.0...
- Installed hashicorp/aws v3.8.0 (signed by HashiCorp)
```

----

### Plan

```bash
$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:
...
Plan: 8 to add, 0 to change, 0 to destroy.
```

----

### Apply

```bash
$ terraform apply
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:
...
Plan: 8 to add, 0 to change, 0 to destroy.
...
Apply complete! Resources: 8 added, 0 changed, 0 destroyed.
```

----

### Destroy

```bash
$ terraform destroy
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:
...
Plan: 0 to add, 0 to change, 8 to destroy.
...
Destroy complete! Resources: 8 destroyed.
```

----

### Format

Mise en forme du code, pour plus de lisibilité

```bash
$ terraform fmt
```

----

### Show

Génération d'un JSON représentant l'état de Terraform

```bash
$ terraform show -json
```
