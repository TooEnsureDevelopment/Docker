#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY platon/Server/platon.Server.csproj platon/Server/
COPY platon/Client/platon.Client.csproj platon/Client/
COPY platon/Shared/platon.Shared.csproj platon/Shared/
RUN dotnet restore "platon/Server/platon.Server.csproj"
COPY . .
WORKDIR "/src/platon/Server"
RUN dotnet build "platon.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "platon.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "platon.Server.dll"]
