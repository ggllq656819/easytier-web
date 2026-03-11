FROM alpine:latest

ARG VERSION=latest
ARG TARGETARCH
ARG TARGETVARIANT

RUN apk add --no-cache tzdata tini bash ca-certificates gcompat libgcc libstdc++

COPY scripts/download.sh /download.sh
RUN apk add --no-cache --virtual .dl-deps curl unzip && \
    sed -i 's/\r$//' /download.sh && \
    chmod +x /download.sh && \
    /bin/bash /download.sh "$VERSION" "$TARGETARCH" "$TARGETVARIANT" && \
    rm -f /download.sh && \
    apk del .dl-deps

COPY --chmod=755 scripts/run.sh /run.sh

RUN mkdir -p /app/data/config
VOLUME /app/data
WORKDIR /app/data

ENTRYPOINT ["/sbin/tini", "--", "/run.sh"]
