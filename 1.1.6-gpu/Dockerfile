FROM nvidia/cuda
MAINTAINER Yasuhiro Matsunaga <ymatsunaga@riken.jp>

#############
USER root

ENV DEBIAN_FRONTEND noninteractive
ENV GENESIS_VERSION 1.1.6
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

# Add Tini
ENV TINI_VERSION v0.16.1
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/local/bin/tini
RUN chmod +x /usr/local/bin/tini

# Configure environment
ENV DEBIAN_FRONTEND=noninteractive \
    OMP_NUM_THREADS=1 \
    AMBERHOME=/opt/amber16 \
    SHELL=/bin/bash \
    NB_USER=jovyan \
    NB_UID=1000 \
    NB_GID=100 \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8 \
    CUDA_HOME=/usr/local/cuda

ENV HOME=/home/$NB_USER

ADD fix-permissions /usr/local/bin/fix-permissions
COPY start.sh /usr/local/bin/

RUN useradd -m -s /bin/bash -N -u $NB_UID $NB_USER && \
    fix-permissions $HOME

RUN mkdir -p /opt && \
    chown $NB_USER:$NB_GID /opt && \
    fix-permissions /opt

WORKDIR /opt
RUN wget --content-disposition "https://www.r-ccs.riken.jp/labs/cbrt/?smd_process_download=1&download_id=6323" \
  && tar jxvf genesis-$GENESIS_VERSION.tar.bz2
WORKDIR genesis-$GENESIS_VERSION/src/
RUN ./configure --enable-single --enable-gpu && make install
RUN ln -s /opt/genesis-$GENESIS_VERSION /opt/genesis

RUN mkdir -p /work && \
    chown $NB_USER:$NB_GID /work && \
    fix-permissions /work

#############
# Switch back to jovyan to avoid accidental container runs as root
USER $NB_USER

ENV PATH /opt/genesis/bin:$PATH
RUN fix-permissions /work
WORKDIR /work

# Configure container startup
ENTRYPOINT ["tini", "--"]
CMD ["start.sh"]

