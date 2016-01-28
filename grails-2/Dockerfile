FROM java:7
MAINTAINER Manuel Ortiz Bey <ortiz.manuel@mozartanalytics.com>

# Set customizable env vars defaults.
# Set Grails version (max version for this Docker image is: 2.5.3).
ENV GRAILS_VERSION 2.5.3

# Install Grails
WORKDIR /usr/lib/jvm
RUN wget https://github.com/grails/grails-core/releases/download/v$GRAILS_VERSION/grails-$GRAILS_VERSION.zip && \
    unzip grails-$GRAILS_VERSION.zip && \
    rm -rf grails-$GRAILS_VERSION.zip && \
    ln -s grails-$GRAILS_VERSION grails

# Setup Grails path.
ENV GRAILS_HOME /usr/lib/jvm/grails
ENV PATH $GRAILS_HOME/bin:$PATH

# Create App Directory
RUN mkdir /app

# Set Workdir
WORKDIR /app

# Set Default Behavior
ENTRYPOINT ["grails"]
