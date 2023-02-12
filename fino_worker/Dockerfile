#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:6.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["fino_worker/fino_worker.csproj", "fino_worker/"]
RUN dotnet restore "fino_worker/fino_worker.csproj"
COPY . .
WORKDIR "/src/fino_worker"
RUN dotnet build "fino_worker.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "fino_worker.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "fino_worker.dll"]