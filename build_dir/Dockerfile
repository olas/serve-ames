FROM ghcr.io/arosbio/cpsign-cp-clf-server:latest-full

LABEL org.opencontainers.image.source=https://github.com/olas/serve-ames
LABEL org.opencontainers.image.description="Ames model cpsign"
LABEL org.opencontainers.image.url=https://github.com/olas/serve-ames
LABEL org.opencontainers.image.version=0.1
LABEL org.opencontainers.image.authors="Ola Spjuth"
LABEL org.opencontainers.image.title="AMES CPSign model"

# Copy the model itself to the image
COPY ames-trained-grad.jar /var/lib/jetty/model.jar

# Change to root user to create a new user and change directory ownership
USER root

ENV USER=jetty2
ENV HOME=/home/$USER

# Create the user 1000
RUN useradd -m -u 1000 $USER

# Set working directory (this is where the code should go)
WORKDIR $HOME/app
# Copy the required startup script 
COPY start-script.sh $HOME/app/start-script.sh
# Make startup script runnable and change user
RUN chmod +x start-script.sh \
    && chown -R $USER:$USER $HOME \
    && chown -R $USER:$USER /var/lib/jetty/ \
    && chown -R $USER:$USER /usr/local/jetty \
    && rm -rf /var/lib/apt/lists/*

# Set the required user
USER $USER

# Set the required entry start-script 
ENTRYPOINT ["./start-script.sh"]