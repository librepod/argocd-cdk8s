VERSION 0.7

ARG --global TARGET_DOCKER_REGISTRY

bun:
  FROM oven/bun:1.0-alpine
  RUN bun --version
  SAVE ARTIFACT /usr/local/bin/bun bun

build:
  ARG ARGOCD_TAG=v2.10.0
  ARG ARGOCD_BASE_IMAGE=quay.io/argoproj/argocd

  FROM ${ARGOCD_BASE_IMAGE}:${ARGOCD_TAG}
  LABEL org.opencontainers.image.source="https://github.com/librepod/${TARGET_DOCKER_REGISTRY}"

  # Switch to root to install stuff
  USER root

  WORKDIR /home/argocd

  RUN apt-get update && apt-get --no-install-recommends --yes install \
      jq curl \
    && curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g cdk8s-cli \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

  COPY +bun/bun /usr/local/bin/bun

  # Switch back to non-root user
  USER 999

  SAVE IMAGE --push ${TARGET_DOCKER_REGISTRY}/argocd-cdk8s:$ARGOCD_TAG
