# docker-mono-apache

Build configuration for running ASP.NET applications within Linux
containers using Mono.

ASP.NET application need not be ASP.NET Core, and image can be run
within Linux container rather than Windows

## How To Use

1.  Copy your published ASP.NET application into the `build` folder
within this project. Your `web.config` should be in the root of `build`

2.  Build and tag for docker. For example:

        docker build -t mywebservicename .
        
3.  Run the image. For example:

        docker run -it -p 8080:80 mywebservicename
        
4.  Test the image locally by accessing <http://localhost:8080/>

## Using Dynamic Web.config Files

With the default setup if `Web.config` changes the image be be rebuilt
and the container restarted. Using volumes it is possible to
dynamically change the configuration without a rebuild / restart.

The difficulty is the volume mounts occur on a folder basis not file
so the settings that you want to be dynamic must be moved into a
subfolder and referenced by `Web.config`.

This is an example for `<appSettings>` but should work for most
sections of `Web.config`.

1.  In `build` folder create a new folder `config`

2.  Within the new folder copy your `<appSettings>` from `Web.config`:

        <appSettings>
          <add key="foo" value="bar" />
        </appSettings> 

3.  Remove `<appSettings>` from `Web.config` and replace with:

        <appSettings configSource="config/appSettings.config">
        
        </appSettings>
          
4.  Ignore `build\config` within `.dockerignore`:

        Dockerfile
        build/config/*
        
5.  Build and tag for docker. For example:
    
        docker build -t mywebservicename .
        
6.  Run the image mounting the `config` folder:

        docker run -it -p 8080:80 -v $(pwd)/build/config/:/var/www/config/ mywebservicename
        

If kubernetes is used, a ConfigMap can be created that will mount in
`/var/www/config/` allowing configuration to be updated without
restarting or rebuilding the pod