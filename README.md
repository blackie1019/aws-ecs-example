# aws-ecs-example
AWS ECS sample code

## Setup ##

接著先確定本機器環境可以執行與運作 .NET Core，並透過以下指令建立一個新的專案

  mkdir webapp
  cd webapp
  dotnet new reactredux

建立好後即可透過以下指令還原套件運行看網站內容：

  dotnet restore
  npm install
  dotnet run

## Build Container Imager ##

完成後我們即可以透過發佈指令將檔案發佈至 *app* 資料夾內：

  dotnet publish --output D:/2.Repo/aws-ecs-example/webapp/app/ --configuration Release

將檔案準備好後，在至 *webapp* 資料夾內新增　*Dockerfile*(抓取 docker container　的映像檔案，參考官方[microsoft/aspnetcore-build](https://hub.docker.com/r/microsoft/aspnetcore-build/)當前最新版 *microsoft/dotnet:2.0.4-sdk-2.1.3-stretch*) ：

```dockerfile
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
```

然後再添加 *.dockerignore* 檔案:

  # Sample contents of .dockerignore file
  bin/
  obj/
  node_modules/

接著我們在 webapp 路徑下執行指令建置本機 docker image 檔案，並將其給予標記 ：
 
  docker build -t ironman .
  docker tag ironman:latest 728812454107.dkr.ecr.ap-northeast-1.amazonaws.com/ironman:latest

照這邊我們已經將 docker image 準備好了，我們可以透過下面指令在本機運行(跑daemon）：

  docker run -d -p 80:80 -t ironman

或是直接運行偵錯都可：

  docker run -it -p 80:80 ironman