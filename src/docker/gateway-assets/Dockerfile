# Build image
FROM microsoft/dotnet:2.0.3-sdk AS build-env  
WORKDIR /sln

# Copy csproj and restore as distinct layers
COPY ./src/ ./

ENV HTTP_PROXY=http://10.16.3.231:80
ENV HTTPS_PROXY=http://10.16.3.231:80

RUN dotnet restore

RUN dotnet build -c Release --no-restore

RUN dotnet test "./Web.UnitTests/Web.UnitTests.csproj" -c Release --no-build --no-restore

RUN dotnet publish "./Web/Web.csproj" -c Release -o "../dist" --no-restore

#Build the app image
FROM microsoft/aspnetcore:2.0.3  
WORKDIR /app  
ENV ASPNETCORE_ENVIRONMENT Local  
ENTRYPOINT ["dotnet", "GatewayAssets.Web.dll"]  
COPY --from=build-env /sln/dist .  
