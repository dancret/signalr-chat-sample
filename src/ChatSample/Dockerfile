FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-stretch-slim AS base
WORKDIR /app
EXPOSE 80

# copy csproj and restore as distinct layers
FROM mcr.microsoft.com/dotnet/core/sdk:2.2-stretch AS build
WORKDIR /src
COPY ["ChatSample/ChatSample.csproj", "ChatSample/"]
RUN dotnet restore "ChatSample/ChatSample.csproj"
COPY . .
WORKDIR "/src/ChatSample"
RUN dotnet build "ChatSample.csproj" -c Release -o /app

# copy everything else and build
FROM build AS publish
RUN dotnet publish "ChatSample.csproj" -c Release -o /app

# build final runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "ChatSample.dll"]