FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["dotnet-start-from-here/dotnet-start-from-here.csproj", "dotnet-start-from-here/"]
RUN dotnet restore "./dotnet-start-from-here/dotnet-start-from-here.csproj"
COPY . .
WORKDIR "/src/dotnet-start-from-here"
RUN dotnet build "./dotnet-start-from-here.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./dotnet-start-from-here.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "dotnet-start-from-here.dll"]