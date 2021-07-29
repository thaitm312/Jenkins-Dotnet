#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0.8 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0.8 AS build
WORKDIR /src
COPY ["Jenkins-Dotnet.API/Jenkins-Dotnet.API.csproj", "Jenkins-Dotnet.API/"]
RUN dotnet restore "Jenkins-Dotnet.API/Jenkins-Dotnet.API.csproj"
COPY . .
WORKDIR "/src/Jenkins-Dotnet.API"
RUN dotnet build "Jenkins-Dotnet.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Jenkins-Dotnet.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Jenkins-Dotnet.API.dll"]