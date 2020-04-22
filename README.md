# DotNetCoreWithDocker

## Scenario
Create a simple service that returns a list of values, then run the service in a Docker container.

## Install .NET SDK
If already installed run the below command to verify.
> dotnet

If the command runs, printing out information about how to use dotnet, you're good to go.

## Create your service

> dotnet new webapi -o myMicroservice --no-https

> cd myMicroservice

The dotnet command creates a new application of type webapi (that's a REST API endpoint).

The -o parameter creates a directory named myMicroservice where your app is stored.

The --no-https flag creates an app that will run without an HTTPS certificate, to keep things simple for deployment.

The cd myMicroService command puts you into the newly created app directory.

## Run your service
> dotnet run

Once the command completes, browse to http://localhost:5000/WeatherForecast

## Install Docker
Docker is a platform that enables you to combine an app plus its configuration and dependencies into a single, independently deployable unit called a container.

Check that Docker is ready to use
> docker --version

## Add Docker metadata

To run with Docker Image you need a Dockerfile — a text file that contains instructions for how to build your app as a Docker image. A docker image contains everything needed to run your app as a Docker container.

```
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src
COPY myMicroservice.csproj .
RUN dotnet restore
COPY . .
RUN dotnet publish -c release -o /app

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
WORKDIR /app
COPY --from=build /app .
ENTRYPOINT ["dotnet", "myMicroservice.dll"]
```

Optional: Add a .dockerignore file
```
Dockerfile
[b|B]in
[O|o]bj
```
A .dockerignore file reduces the set of files that are used as part of `docker build`. Fewer files will result in faster builds.

## Create Docker image
> docker build -t mymicroservice .

The docker build command uses the Dockerfile to build a Docker image.

The -t mymicroservice parameter tells it to tag (or name) the image as mymicroservice.
The final parameter tells it which directory to use to find the Dockerfile (. specifies the current directory).
You can run the following command to see a list of all images available on your machine, including the one you just created.
> docker images

## Run Docker image
> docker run -it --rm -p 3000:80 --name mymicroservicecontainer mymicroservice

http://localhost:3000/WeatherForecast

Optionally, you can view your container running in a separate command prompt using the following command:
> docker ps
