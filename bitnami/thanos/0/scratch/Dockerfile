FROM bitnami/minideb:bullseye as builder

COPY prebuildfs /

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install required system packages and dependencies
RUN install_packages ca-certificates gzip tar wget
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/thanos-0.27.0-3-linux-amd64-debian-11.tar.gz && \
    echo "50f20eed9fd1c4d0aac2c591615d4e00566347b1e5292e681f79c5f8b65dd4fa  /tmp/bitnami/pkg/cache/thanos-0.27.0-3-linux-amd64-debian-11.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/thanos-0.27.0-3-linux-amd64-debian-11.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/thanos-0.27.0-3-linux-amd64-debian-11.tar.gz

######

FROM scratch

COPY prebuildfs /
COPY rootfs /
COPY --from=builder /opt/bitnami/thanos/bin/thanos /bin/thanos

USER 1001

ENTRYPOINT [ "/bin/thanos" ]
