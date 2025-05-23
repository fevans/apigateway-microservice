﻿FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app


FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["api-gateway/ApiGateway/ApiGateway/ApiGateway.csproj", "ApiGateway/"]
COPY ["local-nuget-packages/", "/local-nuget-packages/"]
COPY ["api-gateway/ApiGateway/ApiGateway/nuget.config", "./"]
RUN dotnet restore "ApiGateway/ApiGateway.csproj"

COPY ["api-gateway/ApiGateway/ApiGateway/", "ApiGateway"]
WORKDIR "/src/ApiGateway"
RUN dotnet build "ApiGateway.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "ApiGateway.csproj" -c $BUILD_CONFIGURATION -o /app/publish 

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ApiGateway.dll"]
