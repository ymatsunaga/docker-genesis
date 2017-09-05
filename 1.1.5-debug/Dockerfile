FROM jupyter/base-notebook
MAINTAINER Yasuhiro Matsunaga <ymatsunaga@riken.jp>

USER root

ENV DEBIAN_FRONTEND noninteractive
ENV GENESIS_VERSION 1.1.5
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
RUN wget --content-disposition "http://www.aics.riken.jp/labs/cbrt/?smd_process_download=1&download_id=6090" \
  && tar jxvf genesis-$GENESIS_VERSION.tar.bz2
WORKDIR genesis-$GENESIS_VERSION/src/
RUN ./configure && make install
RUN ln -s /opt/genesis-$GENESIS_VERSION /opt/genesis

RUN wget "http://www.open-mpi.org/software/ompi/v1.8/downloads/openmpi-1.8.8.tar.gz"
  && tar zxvf openmpi-1.8.8.tar.gz
WORKDIR /opt/openmpi-1.8.8
RUN ./configure CC=gcc CXX=g++ F77=gfortran FC=gfortran --enable-mpi-thread-multiple --prefix=/opt/openmpi-1.8.8
RUN make
RUN make install
RUN ln -s /opt/openmpi-1.8.8 /opt/openmpi

RUN mkdir -p /work && \
    chown $NB_USER:$NB_GID /work && \
    fix-permissions /work

# Switch back to jovyan to avoid accidental container runs as root
USER $NB_USER

ENV PATH /opt/genesis/bin:$PATH
ENV PATH PATH=/opt/openmpi/bin:$PATH
ENV LD_LIBRARY_PATH=/opt/openmpi/lib:$LD_LIBRARY_PATH
RUN fix-permissions /work
WORKDIR /work

