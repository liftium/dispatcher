FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

COPY ["Liftium.Dispatcher/Liftium.Dispatcher.csproj", "Liftium.Dispatcher/"]
WORKDIR /src/Liftium.Dispatcher
RUN dotnet restore "Liftium.Dispatcher.csproj"

COPY . .
WORKDIR "/src/Liftium.Dispatcher"
RUN dotnet build "Liftium.Dispatcher.csproj" -c $BUILD_CONFIGURATION -o /app/build /p:UseAppHost=false

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Liftium.Dispatcher.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Liftium.Dispatcher.dll"]
