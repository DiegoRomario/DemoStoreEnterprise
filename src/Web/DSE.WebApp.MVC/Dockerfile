#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["src/Web/DSE.WebApp.MVC/DSE.WebApp.MVC.csproj", "src/Web/DSE.WebApp.MVC/"]
RUN dotnet restore "src/Web/DSE.WebApp.MVC/DSE.WebApp.MVC.csproj"
COPY . .
WORKDIR "/src/src/Web/DSE.WebApp.MVC"
RUN dotnet build "DSE.WebApp.MVC.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "DSE.WebApp.MVC.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DSE.WebApp.MVC.dll"]