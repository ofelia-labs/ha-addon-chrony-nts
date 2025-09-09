ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base:6.2.0
FROM ${BUILD_FROM}

RUN apt-get update \
  && apt-get install -y --no-install-recommends chrony ca-certificates \
  && update-ca-certificates \
  && rm -rf /var/lib/apt/lists/*

COPY rootfs/ /
