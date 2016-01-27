# Optimized Grails Container #

Docker image that will bootstrap an environment for running a Grails application using Docker optimized base images. By default, it will run the Grails app using the `prod run-app` (or `prod run-war` for Grails 2) directive which is the most optimized way of running Grails for production environments. However, you can easily change the default behaviour for your specific uses (see the _Changing Behaviour_ section for more details on this).

## Technologies / Versions Used
- Grails 3.0.12 (by default; all versions from `2.0.0` up to `3.1.0.RC2` can be specified) 
- Java JDK 7+ (for Grails 2) or 8+ (for Grails 3)
- Tomcat 7+ (for Grails 2) or 8+ (for Grails 3)

## Running Using Defaults ##
By default, the mounted app (mounted by using `-v`) will run using the grails `prod run-app` (or `prod run-war` for Grails 2) command which means that the app will run as if it was in a dedicated Tomcat instance in **production** mode. 

Use the following command to run on default mode (remember to ALWAYS specify your app folder in the `-v` command):

`docker run --rm -v /path/to/your/project:/app:rw -p 80:8080 --name grails mozart/grails` 

> Note: This command will run the Grails container using the latest stable version of Grails 3. 

This default behavior can be altered by using the _tag_ modifier for specifying the Grails image version (see the _Image Tag_ version), by modifying the command when running the image (see the _Run Command_ version) or by editing the default environment variables (see the _Environment Variables_ section). 

## Changing Behavior 
You can change the default behavior of the image by either changing some environment variables or executing a different command rather than the default `run-war` command.

### Image Tag / Version
You can change the default behaviour of the image when building and running it by specifying a different Grails image version using the _tag_ (e.g.:`{repository}/{imageName}:{tag}`) modifier at the end of the image identifier (e.g.: `docker run --rm -v /path/to/your/project:/app:rw -p 80:8080 --name grails mozart/grails:2`)

### Environment Variables 
The image contains the following customizable Grails related environment variables:

 - `GRAILS_VERSION`: Specifies the version of Grails to download (default: `3.0.12`; min: `2.0.0`; max: `3.1.0.RC2`).

> **IMPORTANT:** When using the `GRAILS_VERSION` env var to specify a version less than `3.0.0` you MUST use the image version tag with a value of `2` (e.g.: mozart/grails:2). If not, image building will fail!

### Run Command 
You can execute a different Grails command rather than the `run-app` that is run by default, by specifying the command after the `mozart/grails` image name in the following form:

#### For Grails 2

` -- {grails-command}` - The space after the two dashes is **required**. For example:

`docker run --rm -v /path/to/your/project:/app:rw -p 80:8080 --name grails mozart/grails:2 -- run-app`

#### For Grails 3

` {grails-command}` - For example:

`docker run --rm -v /path/to/your/project:/app:rw -p 80:8080 --name grails mozart/grails:3 run-app`

## Interactive Mode 
You can run the Grails console in interactive mode by executing the following command:

#### For Grails 2:

`docker run --rm -v /path/to/your/project:/app:rw -p 80:8080 --name grails mozart/grails -- --interactive`

#### For Grails 3:

`docker run --rm -v /path/to/your/project:/app:rw -p 80:8080 --name grails mozart/grails --interactive`

## Building the Image 
You can build the image by yourself by executing:

`docker build https://raw.githubusercontent.com/mozart-analytics/grails-docker/master/Dockerfile`

Or by downloading directly from the registry:

`docker pull mozart-analytics/grails:{version|latest}`

## Extra: How to use for your Grails apps 
You can also leverage the use of this image for your internal apps if you want more freedom of customization and speed of initialization. To do this: 

 1. Create a Dockerfile for your app.
 2. Use this image as the `FROM:` image of your app's Dockerfile.
 3. Put your app's Dockerfile on the root of the app's folder.
 4. Build your image using your own custom Dockerfile.

An example of a Dockerfile for a Grails-Hello-World app could be:

```
FROM mozart/grails:3
MAINTAINER Manuel Ortiz Bey <ortiz.manuel@gmail.com>

# Copy code to /app directory
COPY . /app
WORKDIR /app

# Run grails compile to download app dependencies and prep the app to run.
RUN grails compile

# Use `run-app` to start application.
CMD ["run-app"]
```

Then build your Docker image by executing:

`docker build -t "{repo-name}/Grails-Hello-World" .`

And finally run your app as a Docker container by executing:

`docker run -it -p 8080:8080 {repo-name}/Grails-Hello-World`

## References
 - More info about [phusion/baseimage](https://github.com/phusion/baseimage-docker).
 - More info about [Grails](https://grails.org/).

## Credits ##
This image was inspired by the [niaquinto/grails](https://registry.hub.docker.com/u/niaquinto/grails/) image available in the Docker Registry Hub.