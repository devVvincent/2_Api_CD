On part du projet précedent (0_API_DevContainer) et on y rajoute :

[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=devVvincent_2_Api_CD&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=devVvincent_2_Api_CD)

---

## 1. Workflow GitHub Actions – CD

Le workflow CD est déclenché après la réussite du workflow CI de tests via un workflow_run.
Il effectue les étapes suivantes :
- Récupération du code
- Le workflow récupère la version du code issue de la PR ou de la branche main.
- Connexion à GitHub Container Registry (GHCR)
- Build de l’image Docker

Exemple de tag :
```sh
ghcr.io/<utilisateur>/<repository>:latest
ghcr.io/<utilisateur>/<repository>:<commit_sha>
```

- Push de l’image Docker sur GHCR

## 2. Dockerfile

Multistage build :
- Build Stage avec mcr.microsoft.com/dotnet/sdk:8.0
- Runtime Stage avec mcr.microsoft.com/dotnet/aspnet:8.0
- Copie uniquement les fichiers nécessaires pour garder l’image propre et légère.

## 3. Sécurité

- Les credentials pour GHCR sont gérés via secrets.GITHUB_TOKEN.
- Les images Docker ne contiennent que les fichiers nécessaires au runtime.
- Les workflows GitHub Actions ont des permissions restreintes (contents: read, packages: write).

---

## Auteur
devVvincent

## Licence
Ce projet est simplement un projet de démonstration.
