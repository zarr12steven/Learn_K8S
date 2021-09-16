FROM ubuntu:20.04

# Author
# Update: 2021-09-16
LABEL maintainer="Daniel"

# Set locales
RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y locales \
    && sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen \
    && dpkg-reconfigure --frontend=noninteractive locales \
    && update-locale LANG=en_US.UTF-8

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8
ENV LC_ALL en_US.UTF-8

# Set TimeZone
ENV TZ=Asia/Taipei
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y tzdata \
    && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
    && echo $TZ > /etc/timezone

# Install Base Tools
# Package 'python-pip' has no installation candidate on Ubuntu 20.04, use 'python3-pip' to replace it.
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y vim curl wget git tig tree dnsutils net-tools jq rsync zip unzip software-properties-common python3-pip

# Install AWS CLI Requirement Packages
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y groff less

# Upgrade pip
RUN pip install --upgrade pip

# Install Ansible Requirement Software
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y sshpass openssh-client

# Install AWS Cli v2
ARG buildtime_AWSCLIv2_VERSION='2.2.37'
ENV AWSCLIv2_VERSION=${buildtime_AWSCLIv2_VERSION}
RUN curl -s "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-${buildtime_AWSCLIv2_VERSION}.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip \
    && ./aws/install \
    && echo "complete -C '/usr/local/bin/aws_completer' aws" >> /root/.bashrc \
    && rm -rf /awscliv2.zip

# Install Nodejs
ARG buildtime_NODEJS_VERSION='v14.17.6'
ENV NODEJS_VERSION=${buildtime_NODEJS_VERSION}
ENV DISTRO='linux-x64'
RUN mkdir -p /usr/local/lib/nodejs \
    && curl -s "https://nodejs.org/dist/${buildtime_NODEJS_VERSION}/node-${buildtime_NODEJS_VERSION}-${DISTRO}.tar.xz" -o "node-${buildtime_NODEJS_VERSION}-${DISTRO}.tar.xz" \
    && tar -xJvf node-${buildtime_NODEJS_VERSION}-${DISTRO}.tar.xz -C /usr/local/lib/nodejs \
    && echo "export PATH=/usr/local/lib/nodejs/node-${buildtime_NODEJS_VERSION}-${DISTRO}/bin:${PATH}" >> /root/.bashrc \
    && rm -rf /node-${buildtime_NODEJS_VERSION}-${DISTRO}.tar.xz \
    && ln -s /usr/local/lib/nodejs/node-${buildtime_NODEJS_VERSION}-${DISTRO}/bin/node /usr/bin/node \
    && ln -s /usr/local/lib/nodejs/node-${buildtime_NODEJS_VERSION}-${DISTRO}/bin/npm  /usr/bin/npm  \
    && ln -s /usr/local/lib/nodejs/node-${buildtime_NODEJS_VERSION}-${DISTRO}/bin/npx  /usr/bin/npx

# Install LocalStack AWS CLI - awslocal
RUN pip install awscli-local

# Install AWS CDK
RUN npm install -g aws-cdk@next

# Create Work Directory
ENV AWS_CONFIG_FILE=/work/.aws/config
ENV AWS_SHARED_CREDENTIALS_FILE=/work/.aws/credentials
RUN mkdir -p /work/.aws

# Clean apt-get cache
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/*

VOLUME ["/work/"]
WORKDIR /work

