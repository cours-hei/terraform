## Concepts

----

### HCL (Hashicorp Configuration Language)

Langage *"human-readable"* proche du JSON

Décrit les ressource utilisées dans Terraform

----

### Providers

Responsable du cycle de vie de la ressource

Basé sur le CRUD (*Create/Read/Update/Delete*)

Plus de 1375 providers existants :
- AWS, GCP, Azure, ...
- Heroku, OVH, 1&1, ...
- Consul, Chef, Vault, ...
- Docker, Kubernetes, ...
- Gitlab, Github, ...
- MySQL, PostgreSQL, ...

----

### Providers

![Image](https://aurelie-vache.developpez.com/tutoriels/cloud/terraform-gerer-infrastructure-code/images/image-4.png)

----

### Providers

Premier fichier .tf à mettre dans votre projet

```json
provider "aws" {
  region = "eu-central-1"
}
```

Attention aux données sensibles à ne pas commiter dans le fichier

Externaliser les AK/SK

```bash
export AWS_ACCESS_KEY=YOUR_ACCESS_KEY
export AWS_SECRET_ACCESS_KEY=YOUR_SECRET_KEY
export AWS_DEFAULT_REGION=eu-central-1
```

----

### Provisioners

Permet de modifier le contenu d'une ressource distante

A n'utiliser qu'en dernier recours. D'autres outils peuvent être utilisés, notamment Packer

----

### Provisioners

Plusieurs types de provisioners
- File
- Local-exec
- Remote-exec
- Chef
- Habitat
- Puppet
- Salt

----

### Provisioners

```json
  provisioner "remote-exec" {
      inline = [
          "sudo apt update && sudo apt -y full-upgrade"
      ]

      connection {
          type = "ssh"
          host = self.public_ip
          user = "ubuntu"
          private_key = file("~/.ssh/id_rsa")
      }
  }
```

----

### Variables

Possibilité d'externaliser des variables dans un autre fichier .tf (ex. variables.tf)

```json
variable "aws_s3_bucket_terraform" {
  default = "lgu-bucket"
}

variable "tags-tool" {
  default = "Terraform"
}

variable "tags-contact" {
    default = "Laurent Grangeau"
}
```

----

### Variables

Utilisation dans les fichiers .tf

```json
resource "aws_s3_bucket" "lgu-bucket" {
  bucket = "$var.aws_s3_bucket_terraform"
  acl    = "private"

  tags {
    Tool    = "$var.tags-tool"
    Contact = "$var.tags-contact"
  }
}
```

----

### Modules

Utilisé pour créer des composants réutilisables, améliorer l'organisation et traiter les éléments de l'infrastructure comme une boite noire

Groupe de ressource qui prend en entrée des *paramètres* et retournent en sortie des *outputs*

```json
module "lambda" {
  source            = "./lambda/"
  toto              = "$var.aws_s3_bucket_toto_key"
  titi              = "$var.aws_s3_bucket_titi_key"
  tutu              = "$var.aws_s3_bucket_tutu_key"
}
```

----

### Outputs

Les modules peuvent produire des outputs que l'on pourra utiliser dans d'autres ressources

```json
output "authorizer_uri" {
  value = "$aws_lambda_function.lambda_toto.invoke_arn"
}
```

```json
resource "aws_api_gateway_authorizer" "custom_authorizer" {
  name                             = "CustomAuthorizer"
  rest_api_id                      = "$aws_api_gateway_rest_api.toto_api.id"
  authorizer_uri                   = "$module.lambda.authorizer_uri"
  identity_validation_expression   = "Bearer .*"
  authorizer_result_ttl_in_seconds = "30"
}
```

----

### Data sources

Récupère une donnée/ressource existante dans votre infrastructure

Utiliser une source de données est une bonne pratique

En effet, il est bon de privilégier son utilisation plutôt que de coder en dur des ```subnet_id```, ```account_id``` ou autre donnée qui peut être récupérée

```json
# VPC
data "aws_subnet" "subnet_id_1" {
  filter {
    name = "tag:Name"
    values = ["lgu-subnet"]
 }
}
```

----

### Data sources

Utilisation des data sources dans les fichiers .tf

```json
module "lgu-sn" {
    source = "lgu-sn"
    subnet_id_1 = "$data.aws_subnet.subnet_id_1.id"
}
```

----

### Import de ressources

Import de ressources existantes dans les fichiers .tf
Cela permet de réaliser des migrations progressive de l'infrastructure

```bash
terraform import aws_instance.frontend i-abcd1234
```

```json
resource "aws_instance" "frontend" {
  instance_type = "t3.micro"

  tags = {
    Name = "Frontend-HelloWorld"
  }
}
```

----

### State

Snapshot de l'infrastructure depuis le dernier ```terraform apply```

Fichier stocké en local

<img src="https://cdn-images-1.medium.com/max/1024/1*lYFNHNM03biX_95IQMayUw.png" width="512px" />

----

### State

Possibilité de stocker ce fichier dans le cloud

```json
# Backend configuration is loaded early so we can't use variables

terraform {
  backend "s3" {
    region  = "eu-central-1"
    bucket  = "lgu-bucket"
    key     = "state.tfstate"
    encrypt = true
  }
}
```

----

### Workspace

Permet d'apporter une facilité pour gerer les environnements (évite de devoir gerer un fichier de configuration tfstate par envrionnement)

```bash
$ terraform workspace list // The command will list all existing workspaces
$ terraform workspace new <workspace_name> // The command will create a workspace
$ terraform workspace select <workspace_name> // The command select a workspace
$ terraform workspace delete <workspace_name> // The command delete a workspace
```

```bash
$ terraform workspace list
default
* dev
preprod
prod
```

Gestion de l'état de la variable workspace

```json
bucket = "$terraform.workspace == "preprod" ? var.bucket_demo_preprod : var.bucket_demo"
```
