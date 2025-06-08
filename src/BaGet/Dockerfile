FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 50561

ENV ASPNETCORE_URLS=http://+:50561

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["src/BaGet/BaGet.csproj", "src/BaGet/"]
RUN dotnet restore "src/BaGet/BaGet.csproj"
COPY . .
WORKDIR "/src/src/BaGet"
RUN dotnet build "BaGet.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "BaGet.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BaGet.dll"]
