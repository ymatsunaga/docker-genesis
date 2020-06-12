FROM nvidia/cuda
MAINTAINER Yasuhiro Matsunaga <ymatsunaga@riken.jp>

USER root

ENV DEBIAN_FRONTEND noninteractive
ENV GENESIS_VERSION 1.5.0
ENV OMP_NUM_THREADS 1

RUN apt-get update && apt-get install -yq --no-install-recommends \
    binutils \
    bzip2 \
    gcc \
    gfortran \
    g++ \
    make \
    liblapack3 \
    liblapack-dev \
    libopenmpi-dev \
    openssh-client \
    openssh-server \
    openmpi-bin \
    python \
    wget \
    gnuplot-nox \
    vim \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /opt
RUN wget --content-disposition "https://www.r-ccs.riken.jp/labs/cbrt/?smd_process_download=1&download_id=14422" \
  && tar jxvf genesis-$GENESIS_VERSION.tar.bz2
WORKDIR genesis-$GENESIS_VERSION/
RUN ./configure --enable-single --enable-gpu && make install
RUN ln -s /opt/genesis-$GENESIS_VERSION /opt/genesis

ENV PATH /opt/genesis/bin:$PATH
WORKDIR /work

