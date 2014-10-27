# Optimized Grails Container #

Docker image that will bootstrap an environment for running a Grails application using the Docker optimized phusion/baseimage as a base and its related scripts for init and destroy. By default, it will run the Grails app using the `run-war` directive which is the most optimized way of running Grails. 

More info about [phusion/baseimage](https://github.com/phusion/baseimage-docker).

More info about [Grails](https://grails.org/).

## Credits ##
This image was inspired by the [niaquinto/grails](https://registry.hub.docker.com/u/niaquinto/grails/) image available in the Docker Registry Hub.

## Technologies / Versions Used
- Grails 2.4.3 (by default, other versions can be specified as well) 
- Java JDK 7+ 
- Tomcat 7+ 

## Running Using Defaults ##
By default, the attached app (attached by using `-v`) will run using the grails `run-war` command which means that the app will run as if it was in a dedicated Tomcat instance in production mode (the environment can be specified using the `GRAILS_ENV` environment variable). 

Use the following command to run on default mode (remember to ALWAYS specify your app folder in the `-v` command):

`docker run --rm -v /path/to/your/project:/app:rw -p 80:8080 --name grails mozart/grails` 

This default behavior can be altered by modifying the command when running the image or by modifying the default environment variables. Go to the *Changing Behavior* section for more information on customizing run behavior.

## Changing Behavior ##
You can change the default behavior of the image by either changing some environment variables or executing a different command rather than the default `run-war` command.

### Environment Variables ###
The image initializes two default environment variables:

 - `GRAILS_VERSION`: Specifies the version of Grails to download (default: `2.4.3`).
 - `GRAILS_ENV`: Specifies the environment on which to bootstrap and run your Grails app (default: `production`).

### Run Command ###
You can execute a different Grails command rather than the `run-war` that is run by default by specifying the command after the `mozart/grails` image name in the form of ` -- {grails-command}`. The space after the two dashes is **required**.

**Example:** `docker run --rm -v /path/to/your/project:/app:rw -p 80:8080 --name grails mozart/grails -- run-app`

## Interactive Mode ##
You can run the Grails console in interactive mode by executing the following command:

`docker run --rm -v /path/to/your/project:/app:rw -p 80:8080 --name grails mozart/grails -- --interactive`

## Building the Image ##
You can build the image by yourself by executing:
`docker build https://github.com/mozart-analytics/grails-docker`

Or by downloading directly from the registry:
`docker pull mozart-analytics/grails`