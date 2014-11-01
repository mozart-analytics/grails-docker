# Optimized Grails Container #

Docker image that will bootstrap an environment for running a Grails application using the Docker optimized phusion/baseimage as a base and its related scripts for init and destroy. By default, it will run the Grails app using the `prod run-war` directive which is the most optimized way of running Grails. 

More info about [phusion/baseimage](https://github.com/phusion/baseimage-docker).

More info about [Grails](https://grails.org/).

## Credits ##
This image was inspired by the [niaquinto/grails](https://registry.hub.docker.com/u/niaquinto/grails/) image available in the Docker Registry Hub.

## Technologies / Versions Used
- Grails 2.4.4 (by default, other versions can be specified) 
- Java JDK 7+ 
- Tomcat 7+ 

## Running Using Defaults ##
By default, the mounted app (mounted by using `-v`) will run using the grails `prod run-war` command which means that the app will run as if it was in a dedicated Tomcat instance in **production** mode. 

Use the following command to run on default mode (remember to ALWAYS specify your app folder in the `-v` command):

`docker run --rm -v /path/to/your/project:/app:rw -p 80:8080 --name grails mozart/grails` 

This default behavior can be altered by modifying the command when running the image or by modifying the default environment variables. Go to the *Changing Behavior* section for more information on customizing run behavior.

## Changing Behavior ##
You can change the default behavior of the image by either changing some environment variables or executing a different command rather than the default `run-war` command.

### Environment Variables ###
The image initializes the following customizable Grails related environment variables:

 - `GRAILS_VERSION`: Specifies the version of Grails to download (default: `2.4.4`).

### Run Command ###
You can execute a different Grails command rather than the `run-war` that is run by default by specifying the command after the `mozart/grails` image name in the form of ` -- {grails-command}`. The space after the two dashes is **required**. For example:

`docker run --rm -v /path/to/your/project:/app:rw -p 80:8080 --name grails mozart/grails -- run-app`

## Interactive Mode ##
You can run the Grails console in interactive mode by executing the following command:

`docker run --rm -v /path/to/your/project:/app:rw -p 80:8080 --name grails mozart/grails -- --interactive`

## Building the Image ##
You can build the image by yourself by executing:

`docker build https://raw.githubusercontent.com/mozart-analytics/grails-docker/master/Dockerfile`

Or by downloading directly from the registry:

`docker pull mozart-analytics/grails`

## Extra: How to use for your Grails apps ##
You can also leverage the use of this image for your internal apps if you want more freedom of customization and speed of initialization. To do this: 

 1. Create a Dockerfile for your app.
 2. Use this image as the `FROM:` image of your app's Dockerfile.
 3. Put your app's Dockerfile on the root of the app's folder.
 4. Build your image using your own custom Dockerfile.

An example of a Dockerfile for a Grails-Hello-World app could be:

```
FROM mozart/grails
MAINTAINER Manuel Ortiz Bey <ortiz.manuel@gmail.com>

# Copy code to /app directory
COPY . /app
WORKDIR /app

# Download app dependencies
RUN grails refresh-dependencies

# Use `run-app` instead of image's default run-war.
CMD ["run-app"]
```

Then build your Docker image by executing:

`docker build -t "{repo-name}/Grails-Hello-World" .`

And finally run your app as a Docker container by executing:

`docker run -i -t -p 8080:8080 {repo-name}/Grails-Hello-World`