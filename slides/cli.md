## CLI (Command Line Interface)

----

### Init

Initialise le répertoire de travail qui contient les fichiers de configuration au format .tf

C'est la première commande à exécuter pour une nouvelle configuration, ou après avoir fait un checkout d'une configuration existante, depuis un dépôt git par exemple

----

### Init

La commande init va :
- télécharger et installer les fournisseurs (providers)
- initialiser le backend (si défini)
- télécharger et installer les modules (si définis)

```bash
$ terraform init
```

----

### Plan

Permet de créer un plan d'exécution. Terraform va déterminer quelles actions il doit entreprendre afin d'avoir les ressources listées dans les fichiers de configuration par rapport à ce qui est actuellement en place sur l'environnement/le fournisseur cible

----

### Plan

Cette commande n'effectue concrètement rien sur l'infrastructure

```bash
$ terraform plan -out plan.out
```

----

### Apply

Permet d'appliquer les changements à effectuer sur l'infrastructure. C'est cette commande qui va créer les ressources

```bash
$ terraform apply plan.out
```

----

### Providers

Permet de vérifier l'utilisation des providers dans les projets et modules Terraform

```bash
$ terraform providers
.
├── provider.aws ~> 1.54.0
└── module.my_module
├── provider.aws (inherited)
└── provider.external
```

----

### Destroy

Permet de supprimer TOUTES les ressources gérées par Terraform

```bash
$ terraform destroy
```

Un plan de suppression peut être généré au préalable

```bash
$ terraform plan -destroy
```

----

### Console

Permet de connaître la valeur d'une ressource Terraform. Cette commande est pratique pour faire du débogage, avant de créer un plan ou de l'appliquer

```bash
$ echo "aws_iam_user.lgu.arn" | terraform console
arn:aws:iam::123456789123:user/lgrangeau
```

----

### Get

Permet de récupérer ou mettre à jour un module

```bash
$ terraform get
```

----

### Graph

Permet de dessiner un graphique de dépendances visuel des ressources Terraform en fonction des fichiers de configuration

```bash
terraform graph | dot -Tpng > graph.png`
```

<img src="https://aurelie-vache.developpez.com/tutoriels/cloud/terraform-gerer-infrastructure-code/images/image-6.png" width="460px" />
