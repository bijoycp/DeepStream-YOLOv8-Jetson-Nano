FROM nvcr.io/nvidia/deepstream-l4t:6.0.1-triton
# Add open GL libraries
RUN apt-get update && \
        DEBIAN_FRONTEND=noninteractive  apt-get install -y --no-install-recommends \
        pkg-config \
        libgnutls28-dev \
        libboost-all-dev && \
        rm -rf /var/lib/apt/lists/* && \
        apt autoremove

WORKDIR /app

ENV LD_LIBRARY_PATH /usr/local/cuda-10.2/lib64
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES all
