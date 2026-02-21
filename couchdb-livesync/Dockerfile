ARG BUILD_FROM
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install CouchDB and dependencies
RUN apk add --no-cache \
    couchdb \
    curl \
    ca-certificates

# Create data directories
RUN mkdir -p /data/couchdb \
    && chown -R couchdb:couchdb /data/couchdb

# Copy root filesystem
COPY rootfs /

# Set up CouchDB data directory symlink
RUN rm -rf /var/lib/couchdb \
    && ln -s /data/couchdb /var/lib/couchdb

# Expose CouchDB port
EXPOSE 5984

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
