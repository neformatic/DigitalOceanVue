#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["digitaloceanweb.client/nuget.config", "digitaloceanweb.client/"]
COPY ["DigitalOceanWeb.Server/DigitalOceanWeb.Server.csproj", "DigitalOceanWeb.Server/"]
COPY ["digitaloceanweb.client/digitaloceanweb.client.esproj", "digitaloceanweb.client/"]
RUN dotnet restore "./DigitalOceanWeb.Server/DigitalOceanWeb.Server.csproj"
COPY . .
WORKDIR "/src/DigitalOceanWeb.Server"
RUN dotnet build "./DigitalOceanWeb.Server.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./DigitalOceanWeb.Server.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DigitalOceanWeb.Server.dll"]