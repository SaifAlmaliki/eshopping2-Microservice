FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["eShopping2/Blazor.UI/Blazor.UI.csproj", "eShopping2/Blazor.UI/"]
RUN dotnet restore "./eShopping2/Blazor.UI/Blazor.UI.csproj"
COPY . .
WORKDIR "/src/eShopping2/Blazor.UI"
RUN dotnet build "./Blazor.UI.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Blazor.UI.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Blazor.UI.dll"]