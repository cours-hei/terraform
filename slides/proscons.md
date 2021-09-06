## Comparaisons et avantages/inconvénients

----

### Comparaison : Terraform

- Support de presque tous les services AWS et d'autres (cloud) providers
- Open source
- HCL/JSON
- State management

----

### Comparaison : Cloudformation

- Support de presque tous les services AWS (uniquement ce cloud provider)
- Service géré et « offert par AWS »
- Le rolling update d'EC2 par un ASG est supporté
- JSON / YAML (depuis 2016)
- Pas de State management

----

### Comparaison : CDK

- Support de presque tous les services AWS (uniquement ce cloud provider)
- Open source
- State management
- Pas d'import de ressource existante
- Support de langage de développement (TS, JS, Python, Go, C#)

----

### Comparaison : Pulumi

- Support de presque tous les services AWS et d'autres (cloud) providers
- Pas de gestion d'erreurs
- State management
- Support de langage de développement (TS, JS, Python, Go, C#)

----

### Avantages

- Permet de définir de l'Infrastructure as Code (IaC)
- Syntaxe concise et lisible
- Réutilisation de : variables, inputs, outputs, modules
- Commande plan
- Multi cloud support
- Développement très actif

----

### Inconvenients

- Pas de rollback possible
- Assez verbeux
- Pauvre gestion des secrets
