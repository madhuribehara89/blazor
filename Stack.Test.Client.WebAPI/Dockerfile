FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM microsoft/dotnet:2.1-sdk AS build
WORKDIR /src
COPY ["Stack.Test.Client.WebAPI/Stack.Test.Client.WebAPI.csproj", "Stack.Test.Client.WebAPI/"]
RUN dotnet restore "Stack.Test.Client.WebAPI/Stack.Test.Client.WebAPI.csproj"
COPY . .
WORKDIR "/src/Stack.Test.Client.WebAPI"
RUN dotnet build "Stack.Test.Client.WebAPI.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "Stack.Test.Client.WebAPI.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Stack.Test.Client.WebAPI.dll"]