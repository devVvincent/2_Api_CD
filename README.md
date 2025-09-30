Continuous Deployment (CD) – 2_API_CD

Ce document décrit le processus de déploiement continu pour le projet 2_API_CD. Il se concentre sur la création et la publication des images Docker après validation des PR.

## 1. Workflow GitHub Actions – CD

Le workflow CD est déclenché après la réussite du workflow CI de tests via un workflow_run. Il effectue les étapes suivantes :
Récupération du code
Le workflow récupère la version du code issue de la PR ou de la branche main.
Connexion à GitHub Container Registry (GHCR)
Build de l’image Docker

Exemple de tag :

ghcr.io/<utilisateur>/<repository>:latest
ghcr.io/<utilisateur>/<repository>:<commit_sha>


Push de l’image Docker sur GHCR
L’image est envoyée sur GitHub Container Registry pour être utilisée dans différents environnements (Dev, Staging, Prod).

## 2. Dockerfile

Multistage build :
Build Stage avec mcr.microsoft.com/dotnet/sdk:8.0
Runtime Stage avec mcr.microsoft.com/dotnet/aspnet:8.0
Copie uniquement les fichiers nécessaires pour garder l’image propre et légère.

## 3. Sécurité

Les credentials pour GHCR sont gérés via secrets.GITHUB_TOKEN.
Les images Docker ne contiennent que les fichiers nécessaires au runtime.
Les workflows GitHub Actions ont des permissions restreintes (contents: read, packages: write).

---

## Auteur
devVvincent

## Licence
Ce projet est simplement un projet de démonstration.
