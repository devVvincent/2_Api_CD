# STAGE 1: Build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Note: On utilise pas de DockerIgnore pour le moment car interfere avec le devcontainer, on utilisera plus tard 2 projets
# Copie csproj et restore en cache
COPY MyApi/MyApi.csproj ./
RUN dotnet restore

COPY MyApi/. . 
RUN dotnet publish -c Release -o /app --no-restore

# STAGE 2: Runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app .

EXPOSE 5000
ENTRYPOINT ["dotnet", "MyApi.dll"]
