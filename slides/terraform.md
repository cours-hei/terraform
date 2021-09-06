## Terraform

----

### Qu'est-ce que Terraform ?

![Image](https://i.pinimg.com/originals/f4/54/15/f45415270449af33c39dcb1e8af5a62a.png)

Créé en 2014 par Hashicorp

Ecrit en Go

Outil qui permet de *construire*, *modifier* et *versionner* une infrastructure

----

### Que fait l'outil ?

- Il permet de provisionner une infrastructure As Code
- Il assure la création et la cohérence d'infrastructure
- Il permet d'appliquer des modifications incrémentales
- On peut détruire des ressources si besoin
- On peut prévisualiser les modifications avant de les appliquer

----

### Trois étapes

- *Write* : écriture de la définition de ses ressources dans des fichiers au format *.tf
- *Plan* : un plan des ressources à créer/modifier & supprimer est affiché, avant tout changement
- *Create* : l'infrastructure voulue est mise en place, et reproductible dans tous les environnements souhaités

----

### Exemple

```json
resource "aws_s3_bucket" "lgu-bucket" {
  bucket = "lgu-bucket"
  acl    = "private"

  tags {
    Owner = "Laurent Grangeau"
  }
}
```
