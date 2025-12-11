FROM ubuntu:22.04

ARG GID=1000
ARG UID=1000

RUN apt-get update \
    && apt-get dist-upgrade -y \
    && apt-get install -y software-properties-common \
    && add-apt-repository -y ppa:ubuntu-toolchain-r/test \
    && add-apt-repository -y ppa:deadsnakes/ppa \
    && apt-get update \
    && apt-get install -y \
         adduser \
         git \
	 g++-13 \
         curl \
         make \
         python3.10 \
         ccache \
         xz-utils \
         g++-14-loongarch64-linux-gnu \
         gcc-14-loongarch64-linux-gnu-base \
         gcc-14-loongarch64-linux-gnu

RUN (addgroup --gid $GID node || groupmod -n node $(getent group $GID | cut -d: -f1)) \
    && (adduser --gid $GID --uid $UID --disabled-password --gecos "" node || usermod -l node -g $GID -d /home/node -m $(getent passwd $UID | cut -d: -f1))

RUN apt-get update \
    && apt-get dist-upgrade -y \
    && apt-get update \
    && apt-get install -y \
         git \
         g++-12 \
         curl \
         make \
         python3 \
         python3-distutils \
         ccache \
         xz-utils

RUN mkdir -p /opt/x-tools \
    && curl -L https://github.com/loong64/cross-tools/releases/download/canadian-20250803/x86_64-linux-gnu-cross-tools-loongarch64-unknown-linux-gnu-oldworld.tar.xz | tar xJf - -C /opt/x-tools

COPY --chown=node:node run.sh /home/node/run.sh

VOLUME /home/node/.ccache
VOLUME /out
VOLUME /home/node/node.tar.xz

USER node

ENTRYPOINT [ "/home/node/run.sh" ]
