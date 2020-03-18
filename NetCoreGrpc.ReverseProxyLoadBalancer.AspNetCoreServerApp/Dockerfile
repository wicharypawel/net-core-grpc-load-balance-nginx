#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 5000
ENV ASPNETCORE_URLS http://*:5000
ENV ASPNETCORE_ENVIRONMENT Development

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["NetCoreGrpc.ReverseProxyLoadBalancer.AspNetCoreServerApp/NetCoreGrpc.ReverseProxyLoadBalancer.AspNetCoreServerApp.csproj", "NetCoreGrpc.ReverseProxyLoadBalancer.AspNetCoreServerApp/"]
RUN dotnet restore "NetCoreGrpc.ReverseProxyLoadBalancer.AspNetCoreServerApp/NetCoreGrpc.ReverseProxyLoadBalancer.AspNetCoreServerApp.csproj"
COPY . .
WORKDIR "/src/NetCoreGrpc.ReverseProxyLoadBalancer.AspNetCoreServerApp"
RUN dotnet build "NetCoreGrpc.ReverseProxyLoadBalancer.AspNetCoreServerApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "NetCoreGrpc.ReverseProxyLoadBalancer.AspNetCoreServerApp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "NetCoreGrpc.ReverseProxyLoadBalancer.AspNetCoreServerApp.dll"]