FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

COPY OrbitBoard.Api.sln .
COPY OrbitBoard.Api/ OrbitBoard.Api/

RUN dotnet restore OrbitBoard.Api.sln
RUN dotnet publish OrbitBoard.Api/OrbitBoard.Api.csproj -c Release -o /app/publish --no-restore

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app

RUN apt-get update \
    && apt-get install -y --no-install-recommends wget \
    && rm -rf /var/lib/apt/lists/*

ENV ASPNETCORE_URLS=http://+:8080

COPY --from=build /app/publish .

EXPOSE 8080

ENTRYPOINT ["dotnet", "OrbitBoard.Api.dll"]
