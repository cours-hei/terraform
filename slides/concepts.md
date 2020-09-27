## Concepts

----

### HCL (Hashicorp Configuration Language)

Langage *"human-readable"* proche du JSON
Décrit les ressource utilisées dans Terraform

----

### Providers

Responsable du cycle de vie de la ressource
Basé sur le CRUD (*Create/Read/Update/Delete*)

Plus de 125 providers existants :
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
  bucket = "${var.aws_s3_bucket_terraform}"
  acl    = "private"

  tags {
    Tool    = "${var.tags-tool}"
    Contact = "${var.tags-contact}"
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
  toto              = "${var.aws_s3_bucket_toto_key}"
  titi              = "${var.aws_s3_bucket_titi_key}"
  tutu              = "${var.aws_s3_bucket_tutu_key}"
}
```

----

### Outputs

Les modules peuvent produire des outputs que l'on pourra utiliser dans d'autres ressources

```json
output "authorizer_uri" {
  value = "${aws_lambda_function.lambda_toto.invoke_arn}"
}
```

```json
resource "aws_api_gateway_authorizer" "custom_authorizer" {
  name                             = "CustomAuthorizer"
  rest_api_id                      = "${aws_api_gateway_rest_api.toto_api.id}"
  authorizer_uri                   = "${module.lambda.authorizer_uri}"
  identity_validation_expression   = "Bearer .*"
  authorizer_result_ttl_in_seconds = "30"
}
```