# On part de l’image officielle .NET SDK 8
FROM mcr.microsoft.com/dotnet/sdk:8.0

# Installer les utilitaires pour useradd/groupeadd et bash
RUN apt-get update && apt-get install -y \
    passwd \
    bash \
    && rm -rf /var/lib/apt/lists/*  

# Créer un nouvel user pour le conteneur
RUN groupadd -g 1000 vscode \
    && useradd -u 1000 -g vscode -m -s /bin/bash vscode

# Se connecte au nouvel user créer
USER vscode
WORKDIR /home/vscode
