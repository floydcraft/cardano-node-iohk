ARG _VERSION=1.27.0
FROM floydcraft/cardano-node-iohk-dev:$_VERSION as base

FROM debian:buster-slim
LABEL maintainer="chbfiv@floydcraft.com"

ARG DEBIAN_FRONTEND=noninteractive

ENV ENV=/etc/profile \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8 \
    NODE_HOME=/opt/cardano-node \
    PATH=/opt/cardano-node/scripts:/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin

USER root
WORKDIR /

COPY --from=base /root/.cabal/bin/* /usr/local/bin/
COPY --from=base /lib/x86_64-linux-gnu/lib* /lib/x86_64-linux-gnu/
COPY --from=base /lib64/ld-linux-x86-64* /lib64/
COPY --from=base /usr/lib/x86_64-linux-gnu/libgmp.* /usr/lib/x86_64-linux-gnu/
COPY --from=base /usr/lib/x86_64-linux-gnu/liblz4.* /usr/lib/x86_64-linux-gnu/
COPY --from=base /usr/lib/x86_64-linux-gnu/libsodium.* /usr/lib/x86_64-linux-gnu/
COPY --from=base /opt/ /opt/
COPY --from=base /config/ /config/

RUN apt-get update \
    && apt-get install --no-install-recommends -y ca-certificates libcap2-bin ncurses-bin iproute2 curl wget xz-utils netbase sudo coreutils dnsutils net-tools procps tcptraceroute bc \
    && apt-get -y clean \
    && apt-get -y autoremove \
    && rm -rf /var/lib/apt/lists/*

ENTRYPOINT [ "bin/bash" ]
