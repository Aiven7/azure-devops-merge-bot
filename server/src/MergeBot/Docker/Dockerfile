
# build
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src

# cache
COPY ../MergeBot.csproj ./
RUN dotnet restore MergeBot.csproj


COPY . ./

# publish
RUN dotnet publish MergeBot.csproj -c Release -o /app/publish

#runtime
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS runtime
WORKDIR /app
EXPOSE 8080
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "MergeBot.dll", "--urls", "http://0.0.0.0:8080"]
