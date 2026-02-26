FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive
ENV MAKEMKV_VERSION=1.18.3

# Base deps + HandBrakeCLI + Intel QSV
RUN apt-get update && apt-get install -y \
    handbrake-cli \
    intel-media-va-driver-non-free \
    vainfo \
    wget \
    build-essential \
    pkg-config \
    libc6-dev \
    libssl-dev \
    libexpat1-dev \
    libavcodec-dev \
    libgl1-mesa-dev \
    qtbase5-dev \
    zlib1g-dev \
    lsscsi \
    && rm -rf /var/lib/apt/lists/*

# Build MakeMKV (CLI only)
WORKDIR /tmp
RUN wget -q https://www.makemkv.com/download/makemkv-oss-${MAKEMKV_VERSION}.tar.gz \
    && wget -q https://www.makemkv.com/download/makemkv-bin-${MAKEMKV_VERSION}.tar.gz \
    && tar xzf makemkv-oss-${MAKEMKV_VERSION}.tar.gz \
    && tar xzf makemkv-bin-${MAKEMKV_VERSION}.tar.gz \
    && cd makemkv-oss-${MAKEMKV_VERSION} \
    && ./configure --disable-gui \
    && make -j$(nproc) \
    && make install \
    && cd /tmp/makemkv-bin-${MAKEMKV_VERSION} \
    && mkdir tmp && echo "accepted" > tmp/eula_accepted \
    && make install \
    && cd / && rm -rf /tmp/*

WORKDIR /data
CMD ["sleep", "infinity"]