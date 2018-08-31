#docker-mono-apache

Build configuration for running ASP.NET applications within Linux
containers using Mono.

ASP.NET application need not be ASP.NET Core, and image can be run
within Linux container rather than Windows

##How To Use

1.  Copy your published ASP.NET application into the `build` folder
within this project. Your `web.config` should be in the root of `build`

2.  Build and tag for docker. For example:

        docker build -t mywebservicename .
        
3.  Run the image. For example:

        docker run -it -p 8080:80 mywebservicename
        
4.  Test the image locally by accessing <http://localhost:8080/>


