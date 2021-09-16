####################################################################################################
#                                                                                                  #
# Maintainer: Daniel Lin                                                                           #
# Date: 2021-09-15                                                                                 #
#                                                                                                  #
####################################################################################################

# Set defaults AWSCLIv2_VERSION, if variable have be setting, will be replaced.
ifndef AWSCLIv2_VERSION
    override AWSCLIv2_VERSION=2.2.38
endif

# Set defaults NODEJS_VERSION, if variable have be setting, will be replaced.
ifndef NODEJS_VERSION
    override NODEJS_VERSION=v14.17.6
endif

# Set defaults CDK_VERSION, if variable have be setting, will be replaced.
ifndef CDK_VERSION
    override CDK_VERSION=2.0.0-rc.21
endif

# Set DockerHub Account Name
ifndef
    override DOCKERHUB_ACCOUNT=daniel
endif

# Get AWSCLIv2 latest tag from github
AWSCLIv2_LATEST=$(shell curl -s "https://api.github.com/repos/aws/aws-cli/tags" | grep 'name' | sed -E 's/.*"([^"]+)".*/\1/' | head -n 1 | cut -c 1-)

.PHONY: build
build: ## Build AWS CDK docker images from Dockerfile, defaults AWSCLIv2_VERSION=2.2.38, NODEJS_VERSION=v14.17.6, CDK_VERSION=2.0.0-rc.21
	@printf "\033[33m=========================%-2sBuild CDK Container, AWSCLIv2_VERSION=${AWSCLIv2_VERSION}, NODEJS_VERSION=${NODEJS_VERSION}%-2s==========================\033[0m\n"
	@docker build --build-arg buildtime_AWSCLIv2_VERSION=$(AWSCLIv2_VERSION) --build-arg buildtime_NODEJS_VERSION=${NODEJS_VERSION} -t $(DOCKERHUB_ACCOUNT)/cdk:$(AWSCLIv2_VERSION) -f Dockerfile .

.PHONY: tag
tag: ## Tag latest for container image, defaults AWSCLIv2_VERSION=2.2.38
ifeq ($(AWSCLIv2_VERSION),$(AWSCLIv2_LATEST))
	@printf "\033[33m============================================%-2sTag $(AWSCLIv2_LATEST) to latest for AWS CDK container%-2s============================================\033[0m\n"
	@docker tag $(DOCKERHUB_ACCOUNT)/cdk:$(AWSCLIv2_VERSION) $(DOCKERHUB_ACCOUNT)/cdk:latest
else ifeq ($(AWSCLIv2_VERSION),$(AWSCLIv2_VERSION))
	@printf "\033[33m===========================================%-2sTag $(AWSCLIv2_VERSION) to latest for AWS CDK container%-2s============================================\033[0m\n"
	@docker tag $(DOCKERHUB_ACCOUNT)/cdk:$(AWSCLIv2_VERSION) $(DOCKERHUB_ACCOUNT)/cdk:latest
endif

.PHONY: run
run: ## Start running AWS CDK container, when you exit container, container will be delete, defaults AWSCLIv2_VERSION=2.2.38, NODEJS_VERSION=v14.17.6, CDK_VERSION=2.0.0-rc.21
ifeq ($(AWSCLIv2_VERSION),$(AWSCLIv2_LATEST))
	@printf "\033[33m=%-2sStart Running AWS CDK Container, defaults AWSCLIv2_VERSION=${AWSCLIv2_VERSION}, NODEJS_VERSION=${NODEJS_VERSION}, CDK_VERSION=${CDK_VERSION}%-2s=\033[0m\n"
	@docker run --name cdk_awscliv2_$(AWSCLIv2_LATEST) -d --volume "${shell PWD}/work:/work" -it $(DOCKERHUB_ACCOUNT)/cdk bash
else ifeq ($(AWSCLIv2_VERSION),$(AWSCLIv2_VERSION))
	@printf "\033[33m=%-2sStart Running AWS CDK Container, defaults AWSCLIv2_VERSION=${AWSCLIv2_VERSION}, NODEJS_VERSION=${NODEJS_VERSION}, CDK_VERSION=${CDK_VERSION}%-2s=\033[0m\n"
	@docker run --name cdk_awscliv2_$(AWSCLIv2_VERSION) -d --volume "${shell PWD}/work:/work" -it $(DOCKERHUB_ACCOUNT)/cdk:$(AWSCLIv2_VERSION) bash
endif

.PHONY: help
help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.DEFAULT_GOAL := help

