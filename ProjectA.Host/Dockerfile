# Use .NET 8.0 SDK for the build environment
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy the project files and restore dependencies
COPY ["ProjectA.Host/ProjectA.Host.csproj", "ProjectA.Host/"]
RUN dotnet restore "./ProjectA.Host/ProjectA.Host.csproj"

# Copy remaining files and build the project
COPY . .
WORKDIR "/src/ProjectA.Host"
RUN dotnet build "./ProjectA.Host.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish the app to a folder
FROM build AS publish
RUN dotnet publish "./ProjectA.Host.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Use .NET 8.0 ASP.NET Core runtime for the final stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Set the entry point for the application
ENTRYPOINT ["dotnet", "ProjectA.Host.dll"]
