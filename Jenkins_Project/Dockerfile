#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Jenkins_Project/Jenkins_Project.csproj", "Jenkins_Project/"]
RUN dotnet restore "Jenkins_Project/Jenkins_Project.csproj"
COPY . .
WORKDIR "/src/Jenkins_Project"
RUN dotnet build "Jenkins_Project.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Jenkins_Project.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Jenkins_Project.dll"]