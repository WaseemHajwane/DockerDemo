# Use official .NET runtime as a base image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Use .NET SDK for building the project
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# 🔹 Corrected COPY command to match actual project structure
COPY ["DockerDemo.csproj", "./"]

# 🔹 Restore NuGet packages
RUN dotnet restore "./DockerDemo.csproj"

# 🔹 Copy all source files correctly
COPY . .

# 🔹 Change working directory to match project structure
WORKDIR "/src"

# 🔹 Build the project
RUN dotnet build "DockerDemo.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish the project
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "DockerDemo.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Start the application
ENTRYPOINT ["dotnet", "DockerDemo.dll"]
