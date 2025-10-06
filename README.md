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
- Push de l’image Docker sur GHCR

Exemple de tag :
```sh
ghcr.io/devvvincent/2-api-cd:latest
ghcr.io/<utilisateur>/<repository>:<commit_sha>
```

## 2. Dockerfile

Multistage build :
- Build Stage avec mcr.microsoft.com/dotnet/sdk:8.0
- Runtime Stage avec mcr.microsoft.com/dotnet/aspnet:8.0
- Copie uniquement les fichiers nécessaires pour garder l’image propre et légère.

3. AWS EC2 et déploiement automatique

L’application est déployée sur une instance EC2 Ubuntu t3.micro et exposée sur le port 5000.

 - Connexion au GHCR
 ```sh
 echo "GHCR_TOKEN" | docker login ghcr.io -u devVvincent --password-stdin
 ```

 - Déploiement du conteneur
 ```sh
 docker run -d \
 --name myapi \
 -e ASPNETCORE_ENVIRONMENT=Development \
 -e ASPNETCORE_URLS=http://+:5000 \
 -p 5000:5000 \
 ghcr.io/devvvincent/2-api-cd:latest
 ```

- Mise à jour automatique avec Watchtower,pour garder le conteneur à jour à chaque nouvelle image sur GHCR :
 ```sh
 docker run -d \
 --name watchtower \
 -v /var/run/docker.sock:/var/run/docker.sock \
 containrrr/watchtower \
 myapi \
 --interval 60 \
 --cleanup
 ```
Watchtower surveille le conteneur myapi et le met à jour automatiquement dès qu’une nouvelle image est disponible.

## 4. Sécurité

- Les credentials pour GHCR sont gérés via secrets.GITHUB_TOKEN.
- Les images Docker ne contiennent que les fichiers nécessaires au runtime.
- Les workflows GitHub Actions ont des permissions restreintes (contents: read, packages: write).

---

## Auteur
devVvincent

## Licence
Ce projet est simplement un projet de démonstration.
