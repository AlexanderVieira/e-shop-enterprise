#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["src/Services/Payment/ESE.Payment.API/ESE.Payment.API.csproj", "src/Services/Payment/ESE.Payment.API/"]
RUN dotnet restore "src/Services/Payment/ESE.Payment.API/ESE.Payment.API.csproj"
COPY . .
WORKDIR "/src/src/Services/Payment/ESE.Payment.API"
RUN dotnet build "ESE.Payment.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ESE.Payment.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ESE.Payment.API.dll"]