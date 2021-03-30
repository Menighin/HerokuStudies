#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
#EXPOSE 80
#EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["HerokuCICD/HerokuCICD.csproj", "HerokuCICD/"]
RUN dotnet restore "HerokuCICD/HerokuCICD.csproj"
COPY . .
WORKDIR "/src/HerokuCICD"
RUN dotnet build "HerokuCICD.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "HerokuCICD.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
#ENV ASPNETCORE_ENVIRONMENT=Development
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "HerokuCICD.dll"]