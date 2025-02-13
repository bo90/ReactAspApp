# Этап 1: Сборка React-приложения
FROM node:18 AS react-build
WORKDIR /app

# Копируем файлы React
COPY ClientApp/package.json .
RUN npm install
COPY ClientApp/ .
RUN npm run build

# Этап 2: Сборка и запуск ASP.NET Core
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src

# Копируем .csproj и восстанавливаем зависимости
COPY *.csproj .
RUN dotnet restore

# Копируем исходный код ASP.NET Core
COPY . .

# Копируем собранные React-файлы в папку wwwroot
# COPY --from=react-build /app/ClientApp/build ./wwwroot

# Собираем и публикуем проект
RUN dotnet publish -c Release -o /app/publish

# Финальный образ
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=build /app/publish .
EXPOSE 80
ENTRYPOINT ["dotnet", "MyReactApp.dll"]