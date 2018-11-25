FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM microsoft/dotnet:2.1-sdk AS build
WORKDIR /src
COPY ["Stack.Test.Client.BlazorApp/Stack.Test.Client.BlazorApp.csproj", "Stack.Test.Client.BlazorApp/"]
RUN dotnet restore "Stack.Test.Client.BlazorApp/Stack.Test.Client.BlazorApp.csproj"
COPY . .
WORKDIR "/src/Stack.Test.Client.BlazorApp"
RUN dotnet build "Stack.Test.Client.BlazorApp.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "Stack.Test.Client.BlazorApp.csproj" -c Release -o /app

FROM base AS copy
WORKDIR /app
COPY --from=publish /app /app/minimal

FROM nginx:alpine as final
COPY --from=copy /app/minimal /usr/share/nginx/html/
COPY Stack.Test.Client.BlazorApp/nginx.conf /etc/nginx/nginx.conf