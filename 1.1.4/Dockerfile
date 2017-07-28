FROM ubuntu:16.04

ENV GENESIS_VERSION 1.1.4
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
	&& rm -rf /var/lib/apt/lists/*


WORKDIR /opt
RUN wget --content-disposition "http://www.aics.riken.jp/labs/cbrt/?smd_process_download=1&download_id=5162" \
  && tar jxvf genesis-$GENESIS_VERSION.tar.bz2
WORKDIR genesis-$GENESIS_VERSION/src/
RUN ./configure && make install
RUN ln -s /opt/genesis-$GENESIS_VERSION /opt/genesis

#WORKDIR /opt
#RUN wget --content-disposition "http://www.aics.riken.jp/labs/cbrt/?smd_process_download=1&download_id=4875" \
#  && tar jxvf tests-$GENESIS_VERSION.tar.bz2
#WORKDIR tests-$GENESIS_VERSION/regression_test/
##python test.py "mpirun --allow-run-as-root -np 8 /opt/genesis/bin/spdyn"

ENV PATH /opt/genesis/bin:$PATH
#WORKDIR /work/tests-$GENESIS_VERSION/regression_test/
WORKDIR /work

