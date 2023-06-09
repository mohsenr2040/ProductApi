#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["ProductApi/ProductApi.csproj", "ProductApi/"]
COPY ["ProductApi.Domain/ProductApi.Domain.csproj", "ProductApi.Domain/"]
COPY ["ProductApi.Service/ProductApi.Service.csproj", "ProductApi.Service/"]
COPY ["ProductApi.Messaging.Send/ProductApi.Messaging.Send.csproj", "ProductApi.Messaging.Send/"]
COPY ["ProductApi.Data/ProductApi.Data.csproj", "ProductApi.Data/"]
RUN dotnet restore "ProductApi/ProductApi.csproj"
COPY . .
WORKDIR "/src/ProductApi"
RUN dotnet build "ProductApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ProductApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ProductApi.dll"]