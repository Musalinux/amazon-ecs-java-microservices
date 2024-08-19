# Use a specific version of Alpine for more control over the environment
FROM python:3.10-slim

# Install dependencies
RUN apk update && \
    apk add --no-cache ca-certificates bash groff less python3 py3-pip && \
    update-ca-certificates

# Install AWS CLI v2 directly from AWS
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install

RUN pip3 install --trusted-host pypi.org --trusted-host files.pythonhosted.org awscli  

# Install additional necessary packages
RUN apk add --no-cache \
        btrfs-progs \
        e2fsprogs \
        e2fsprogs-extra \
        iptables \
        xfsprogs \
        xz \
        openjdk8 \
        maven \
        curl \
        git \
        openssh-client \
        zip \
        unzip \
        ttf-dejavu \
        openssl

# Environment variables for Java and AWS
ENV JAVA_HOME=/usr/lib/jvm/java-1.8-openjdk
ENV PATH="$JAVA_HOME/bin:$PATH:/usr/local/bin"

# Additional commands for Docker in Docker configuration
ENV DIND_COMMIT=3b5fac462d21ca164b3778647420016315289034
RUN wget -O /usr/local/bin/dind "https://raw.githubusercontent.com/docker/docker/${DIND_COMMIT}/hack/dind" && \
    chmod +x /usr/local/bin/dind

# Set up Docker in Docker
RUN set -x \
    && addgroup -S dockremap \
    && adduser -S -G dockremap dockremap \
    && echo 'dockremap:165536:65536' >> /etc/subuid \
    && echo 'dockremap:165536:65536' >> /etc/subgid

# Copy the Docker entrypoint script
COPY dockerd-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/dockerd-entrypoint.sh

# Setup volume and expose necessary port for Docker daemon
VOLUME /var/lib/docker
EXPOSE 2375

# Define the entrypoint and command
ENTRYPOINT ["dockerd-entrypoint.sh"]
CMD []
