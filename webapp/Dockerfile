# Stage 2
FROM microsoft/aspnetcore

RUN apt-get -qq update && apt-get -qqy --no-install-recommends install wget gnupg \
    git \
    unzip

RUN curl -sL https://deb.nodesource.com/setup_6.x |  bash -
RUN apt-get install -y nodejs
WORKDIR /app
COPY /app .

ENTRYPOINT ["dotnet", "webapp.dll"]