#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["dockerWebApp/Server/dockerWebApp.Server.csproj", "dockerWebApp/Server/"]
COPY ["dockerWebApp/Client/dockerWebApp.Client.csproj", "dockerWebApp/Client/"]
COPY ["dockerWebApp/Shared/dockerWebApp.Shared.csproj", "dockerWebApp/Shared/"]
RUN dotnet restore "dockerWebApp/Client/dockerWebApp.Client.csproj"
COPY . .
WORKDIR "/src/dockerWebApp/Client"
RUN dotnet build "dockerWebApp.Client.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "dockerWebApp.Client.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "dockerWebApp.Client.dll"]