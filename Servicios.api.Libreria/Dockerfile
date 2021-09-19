#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.
#IDEA: Mover el cod y la ejecucion del cod del proyecto dentro de un container
#Para ello se crea la imagen con este archivo...

#Descarga de runtime net core 5. Incluye compiladores del cod net core
FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
#Codigo autogenerado para la descarga del sdk para net core 5.Cjto de librerias que se requieren para 
#la construccion de la aplicacion.
FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
#Señala la ubicacion en el container donde se almacenara cod fuente del microservicio
COPY ["Servicios.api.Libreria/Servicios.api.Libreria.csproj", "Servicios.api.Libreria/"]
RUN dotnet restore "Servicios.api.Libreria/Servicios.api.Libreria.csproj"
COPY . .
WORKDIR "/src/Servicios.api.Libreria"
RUN dotnet build "Servicios.api.Libreria.csproj" -c Release -o /app/build
#proceso de compilacion y publicacion del microservicio al interior del container
FROM build AS publish
RUN dotnet publish "Servicios.api.Libreria.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Servicios.api.Libreria.dll"]