VERSION 0.7

ARG --global TARGET_DOCKER_REGISTRY

build:
  ARG ARGOCD_TAG=v2.9.6
  ARG ARGOCD_BASE_IMAGE=quay.io/argoproj/argocd

  FROM ${ARGOCD_BASE_IMAGE}:${ARGOCD_TAG}
  LABEL org.opencontainers.image.source="https://github.com/librepod/${TARGET_DOCKER_REGISTRY}"

  # Switch to root for the ability to perform install
  USER root

  RUN apt-get update

  # Setup non-root user
  WORKDIR /home/argocd

  RUN apt-get update && apt-get --no-install-recommends --yes install \
      jq curl \
    && curl -L -o yq_linux_amd64.tar.gz https://github.com/mikefarah/yq/releases/download/v4.2.0/yq_linux_amd64.tar.gz \
    && tar xzf ./yq_linux_amd64.tar.gz \
    && chmod +x ./yq_linux_amd64 && mv ./yq_linux_amd64 /usr/local/bin/yq \
    && curl -fsSL https://deb.nodesource.com/setup_21.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g pnpm cdk8s-cli \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

  # Switch back to non-root user
  USER 999

  SAVE IMAGE --push ${TARGET_DOCKER_REGISTRY}/argocd-cdk8s:$ARGOCD_TAG
