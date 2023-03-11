# use base imange from  mcr.microsoft.com/dotnet/aspnet version 5.0
FROM mcr.microsoft.com/dotnet/aspnet:5.0 as base

# set work directory at /app
WORKDIR /app

# use image from mcr.microsoft.com/dotnet/sdk versin 5.0
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build

# copy all of /aspnetapp to /src in image 
COPY . /src

# set work directory at /src and build asp.net app
WORKDIR /src
RUN ls
RUN dotnet build "aspnetapp.csproj" -c Release -o /app/build

# publish app
FROM build AS publish
RUN dotnet publish "aspnetapp.csproj" -c Release -o /app/publish

# run the application
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "aspnetapp.dll"]