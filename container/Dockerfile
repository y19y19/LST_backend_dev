FROM nvcr.io/nvidia/tritonserver:24.04-py3
# nvcc version: 12.4 ## nvcc --version
# cudnn version: 9.1.0  ## find / -name "libcudnn*" 2>/dev/null

LABEL description="Triton Server backend with other dependencies for LST"
LABEL version="1.0"

# Install dependencies
# Update the CUDA Linux GPG Repository Key
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub

# See also https://root.cern.ch/build-prerequisites
RUN apt-get update -y && apt-get install -y \
    autoconf automake libre2-dev libssl-dev libtool libcurl4-openssl-dev patchelf \
    build-essential curl git freeglut3-dev libfreetype6-dev libpcre3-dev\
    libboost-dev libboost-filesystem-dev libboost-program-options-dev libboost-test-dev \
    libtbb-dev ninja-build time tree \
    python3 python3-dev python3-pip python3-numpy python3-setuptools \
    rsync zlib1g-dev ccache vim unzip libblas-dev liblapack-dev swig \
    rapidjson-dev \
    libexpat-dev libeigen3-dev libftgl-dev libgl2ps-dev libglew-dev libgsl-dev \
    liblz4-dev liblzma-dev libx11-dev libxext-dev libxft-dev libxpm-dev libxerces-c-dev \
    libzstd-dev ccache libb64-dev \
    software-properties-common unzip wget zlib1g-dev pkg-config uuid-dev \
  && apt-get clean -y

RUN ln -s /usr/bin/python3 /usr/bin/python
RUN pip3 install --upgrade pip
RUN pip3 install -U pandas matplotlib seaborn

# Environment variables
ENV LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/lib:/usr/local/lib"
ENV GET="curl --location --silent --create-dirs"
ENV UNPACK_TO_SRC="tar -xz --strip-components=1 --directory src"
ENV PREFIX="/usr/local"
ENV TORCH_CUDA_ARCH_LIST="80"
ENV PYTHONNOUSERSITE=True

# Manual builds for specific packages
# Install CMake v3.29.4
RUN cd /tmp && mkdir -p src \
  && ${GET} https://github.com/Kitware/CMake/releases/download/v3.29.4/cmake-3.29.4-Linux-x86_64.tar.gz \
    | ${UNPACK_TO_SRC} \
  && rsync -ru src/ ${PREFIX} \
  && cd /tmp && rm -rf /tmp/src
