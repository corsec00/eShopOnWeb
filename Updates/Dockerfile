# Estágio 1: BUILD
FROM mcr.microsoft.com/dotnet/sdk:8.0-nanoserver-ltsc2022 AS build
WORKDIR /src
COPY src/*/*.csproj ./
RUN for %F in (ApplicationCore, BlazorAdmin, BlazorShared, Infrastructure, PublicApi, Web) do mkdir %F
RUN move ApplicationCore.csproj ApplicationCore
RUN move BlazorAdmin.csproj BlazorAdmin
RUN move BlazorShared.csproj BlazorShared
RUN move Infrastructure.csproj Infrastructure
RUN move PublicApi.csproj PublicApi
RUN move Web.csproj Web
RUN dotnet restore "Web/Web.csproj"
COPY src/. .

# Estágio 2: PUBLISH
WORKDIR /src/Web
RUN dotnet publish "Web.csproj" -c Release -o /app/publish --no-restore

# Estágio 3: FINAL
FROM mcr.microsoft.com/dotnet/aspnet:8.0-nanoserver-ltsc2022 AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Web.dll"]
