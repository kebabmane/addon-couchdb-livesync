# Use official CouchDB image as base
FROM couchdb:3.4.2

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install s6-overlay
ARG S6_OVERLAY_VERSION="3.2.0.2"
ARG TARGETARCH

RUN apt-get update && apt-get install -y --no-install-recommends \
    xz-utils \
    curl \
    ca-certificates \
    jq \
    && rm -rf /var/lib/apt/lists/*

# Install s6-overlay
RUN S6_ARCH="${TARGETARCH}" \
    && if [ "${S6_ARCH}" = "amd64" ]; then S6_ARCH="x86_64"; fi \
    && if [ "${S6_ARCH}" = "arm64" ]; then S6_ARCH="aarch64"; fi \
    && curl -L -s "https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-noarch.tar.xz" | tar Jxpf - -C / \
    && curl -L -s "https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-${S6_ARCH}.tar.xz" | tar Jxpf - -C /

# Install bashio
RUN curl -L -s "https://github.com/hassio-addons/bashio/archive/refs/tags/v0.16.2.tar.gz" | tar -xzf - \
    && mv bashio-0.16.2/lib /usr/lib/bashio \
    && ln -s /usr/lib/bashio/bashio /usr/bin/bashio \
    && rm -rf bashio-0.16.2

# Create data directories
RUN mkdir -p /data \
    && chown -R couchdb:couchdb /data

# Copy root filesystem
COPY rootfs /

# Make scripts executable
RUN chmod +x /etc/s6-overlay/s6-rc.d/couchdb/run \
    && chmod +x /etc/s6-overlay/s6-rc.d/couchdb/finish

# Expose CouchDB port
EXPOSE 5984

# Environment for s6
ENV S6_BEHAVIOUR_IF_STAGE2_FAILS=2
ENV S6_CMD_WAIT_FOR_SERVICES_MAXTIME=0

# Labels
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION
LABEL \
    io.hass.name="CouchDB for Obsidian LiveSync" \
    io.hass.description="CouchDB database configured for Obsidian LiveSync plugin synchronization" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version="${BUILD_VERSION}" \
    org.opencontainers.image.title="CouchDB for Obsidian LiveSync" \
    org.opencontainers.image.description="CouchDB database configured for Obsidian LiveSync" \
    org.opencontainers.image.source="https://github.com/kebabmane/addon-couchdb-livesync" \
    org.opencontainers.image.created="${BUILD_DATE}" \
    org.opencontainers.image.revision="${BUILD_REF}" \
    org.opencontainers.image.version="${BUILD_VERSION}" \
    org.opencontainers.image.licenses="MIT"

# Use s6-overlay entrypoint
ENTRYPOINT ["/init"]
