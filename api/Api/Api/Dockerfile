# Dockerfile для .NET 9
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src

# Копируем файл проекта для оптимизации кэша
COPY ["Api.csproj", "."]
RUN dotnet restore

# Копируем остальной код
COPY . .

# Собираем
RUN dotnet build -c Release -o /app/build
RUN dotnet publish -c Release -o /app/publish

# Финальный образ
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS final
WORKDIR /app
COPY --from=build /app/publish .

# Переменные окружения
ENV ASPNETCORE_ENVIRONMENT=Production
ENV ASPNETCORE_URLS=http://+:8080

EXPOSE 8080
ENTRYPOINT ["dotnet", "Api.dll"]