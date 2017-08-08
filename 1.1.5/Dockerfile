FROM ubuntu:16.04

ENV GENESIS_VERSION 1.1.5
ENV OMP_NUM_THREADS 1

RUN apt-get update && apt-get install -y --no-install-recommends \
    binutils \
    bzip2 \
    gcc \
    gfortran \
    g++ \
    make \
    liblapack-dev \
    libopenmpi-dev \
    openssh-client \
    openssh-server \
    openmpi-bin \
    python \
    wget \
    gnuplot-nox \
    vim \
	&& rm -rf /var/lib/apt/lists/*


WORKDIR /opt
RUN wget --content-disposition "http://www.aics.riken.jp/labs/cbrt/?smd_process_download=1&download_id=6090" \
  && tar jxvf genesis-$GENESIS_VERSION.tar.bz2
WORKDIR genesis-$GENESIS_VERSION/src/
RUN ./configure && make install
RUN ln -s /opt/genesis-$GENESIS_VERSION /opt/genesis

RUN groupadd -r yasu && useradd -r -g postgres yasu

USER yasu
ENV PATH /opt/genesis/bin:$PATH
WORKDIR /work

