# Home Assistant add-on base images are Alpine (BusyBox ash)
ARG BUILD_FROM=ghcr.io/home-assistant/${BUILD_ARCH}-base:latest
FROM ${BUILD_FROM}

# Use ash; fail fast
SHELL ["/bin/ash", "-eo", "pipefail", "-c"]

# Install chrony (with NTS support in Alpine builds) and CA roots
RUN apk add --no-cache chrony ca-certificates \
    && update-ca-certificates \
    && mkdir -p /data/chrony \
    && chmod 0700 /data/chrony

# Bring in s6 services and init scripts
COPY rootfs/ /

# s6-overlay entrypoint
CMD ["/init"]
