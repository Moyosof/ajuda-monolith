FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ProjectA.Host/ProjectA.Host.csproj", "ProjectA.Host/"]
RUN dotnet restore "./ProjectA.Host/ProjectA.Host.csproj"
COPY . .
WORKDIR "/src/ProjectA.Host"
RUN dotnet build "./ProjectA.Host.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./ProjectA.Host.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ProjectA.Host.dll"]