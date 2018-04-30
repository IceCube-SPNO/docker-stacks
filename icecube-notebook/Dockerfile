# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
FROM icecube/tensorflow-notebook

LABEL maintainer="Claudio Kopper <ckopper@icecube.wisc.edu>"

# Set when building on Travis so that certain long-running build steps can
# be skipped to shorten build time.
ARG TEST_ONLY_BUILD

USER root

# OpenCL
RUN apt-get update && apt-get install -y --no-install-recommends \
        ocl-icd-libopencl1 \
        ocl-icd-opencl-dev \
        clinfo && \
        apt-get clean && \
    rm -rf /var/lib/apt/lists/*
RUN mkdir -p /etc/OpenCL/vendors && \
    echo "libnvidia-opencl.so.1" > /etc/OpenCL/vendors/nvidia.icd

# additional tools
RUN apt-get update && apt-get install -y --no-install-recommends \
        subversion \
        nano \
        less \
        rsync \
        openssh-client \
        man-db \
        lsb-release \
        gzip \
        bzip2 \
        xz-utils \
        liblzma5 \
        liblzma-dev \
        zlib1g \
        zlib1g-dev \
        build-essential \
        mc && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

USER $NB_UID

# cling, cmake
RUN conda install --quiet --yes -c QuantStack -c conda-forge \
    'xeus-cling=0.4*' && \
    conda install --quiet --yes \
    'cmake=3.11*' \
    'boost=1.67*' \
    'boost-cpp=1.67*' \
    'cfitsio=3.430*'&& \
    conda clean -tipsy && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

