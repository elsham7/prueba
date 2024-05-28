#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["prueba/prueba.csproj", "prueba/"]
RUN dotnet restore "./prueba/prueba.csproj"
COPY . .
WORKDIR "/src/prueba"
RUN dotnet build "./prueba.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./prueba.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

VOLUME /etc/docker

ENV ASPNET_HTTPS_PORT=5003
ENV ASPNET_HTTPS_KEY_FILE=/etc/aspnet/ssl/clave.pem
ENV ASPNET_HTTPS_CERTIFICATE_FILE=/etc/aspnet/ssl/certificado.pem

#COPY /etc/docker/certificado.p12 /usr/local/share/dotnet/store/mykeystore.p12
#ENV ASPNETCORE_URLS https://*:5003
#ENV DOTNET_USE_SYSTEM_CERTIFICATE=true
#ENV DOTNET_SYSTEM_CERTIFICATE_FILE=/usr/local/share/dotnet/store/mykeystore.p12
#ENV DOTNET_SYSTEM_CERTIFICATE_STORE=My
#ENV DOTNET_SYSTEM_CERTIFICATE_PASSWORD=Ecuador123
#
#ENV ASPNET_HTTPS_PORT=443
#ENV ASPNET_HTTPS_KEY_FILE=/etc/aspnet/ssl/clave.pem
#ENV ASPNET_HTTPS_CERTIFICATE_FILE=/etc/aspnet/ssl/certificado.pem


FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "prueba.dll"]