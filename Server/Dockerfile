#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Server/BlazorTest01.Server.csproj", "Server/"]
COPY ["Client/BlazorTest01.Client.csproj", "Client/"]
COPY ["Shared/BlazorTest01.Shared.csproj", "Shared/"]
RUN dotnet restore "Server/BlazorTest01.Server.csproj"
COPY . .
WORKDIR "/src/Server"
RUN dotnet build "BlazorTest01.Server.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
RUN dotnet publish "BlazorTest01.Server.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorTest01.Server.dll"]