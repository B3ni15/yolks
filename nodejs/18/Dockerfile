FROM --platform=$TARGETOS/$TARGETARCH node:18-bookworm-slim

LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

RUN apt update \
    && apt -y install ffmpeg iproute2 git sqlite3 libsqlite3-dev python3 python3-dev ca-certificates dnsutils tzdata zip tar curl build-essential libtool iputils-ping libnss3 tini \
    && useradd -m -d /home/container container

RUN npm install npm@10.x.x typescript ts-node @types/node --location=global

RUN npm install -g corepack
RUN corepack enable --install-directory=/usr/local/bin
RUN corepack prepare pnpm@8.0.0 --activate

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

STOPSIGNAL SIGINT

COPY --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]