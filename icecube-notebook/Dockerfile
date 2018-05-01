# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
FROM icecube/tensorflow-notebook

LABEL maintainer="Claudio Kopper <ckopper@icecube.wisc.edu>"

# Set when building on Travis so that certain long-running build steps can
# be skipped to shorten build time.
ARG TEST_ONLY_BUILD

USER root

# additional tools
RUN apt-get update && apt-get install -y --no-install-recommends \
        subversion \
        rsync && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# add a missing group (same name as the username, same id as the UID)
RUN addgroup --gid $NB_UID $NB_USER

# create the directory for IceCube test data
RUN mkdir /opt/i3-data && chown $NB_UID:$NB_GID /opt/i3-data

USER $NB_UID

# download test and runtime data
ENV I3_DATA /opt/i3-data

# install some additional data (clsim and nugen)
RUN mkdir /opt/i3-data/neutrino-generator && \
    wget -nv -t 5 -O /opt/i3-data/safeprimes_base32.gz  http://code.icecube.wisc.edu/tools/clsim/safeprimes_base32.gz && \
    wget -nv -t 5 -O /opt/i3-data/safeprimes_base32.txt http://code.icecube.wisc.edu/tools/clsim/safeprimes_base32.txt && \
    wget -nv -t 5 -O /opt/i3-data/neutrino-generator/nugen-v3-tables.tgz http://code.icecube.wisc.edu/tools/neutrino-generator/nugen-v3-tables.tgz && \
    tar -zx -f /opt/i3-data/neutrino-generator/nugen-v3-tables.tgz -C /opt/i3-data/neutrino-generator && \
    rm /opt/i3-data/neutrino-generator/nugen-v3-tables.tgz && \
    chmod -R u+rwX,go+rX,go-w /opt/i3-data/neutrino-generator

# add GCD files
RUN mkdir /opt/i3-data/GCD && \
    wget -nv -N -t 5 -P /opt/i3-data/GCD/ -r -l 1 -A *.i3* -nd http://prod-exe.icecube.wisc.edu/GCD/ && \
    chmod -R u+rwX,go+rX,go-w /opt/i3-data/GCD

# install photon tables
RUN mkdir /opt/i3-data/photon-tables && \
    mkdir /opt/i3-data/photon-tables/splines && \
    wget -nv -t 5 -O /opt/i3-data/photon-tables/splines/ems_mie_z20_a10.abs.fits       http://prod-exe.icecube.wisc.edu/spline-tables/ems_mie_z20_a10.abs.fits && \
    wget -nv -t 5 -O /opt/i3-data/photon-tables/splines/ems_mie_z20_a10.prob.fits      http://prod-exe.icecube.wisc.edu/spline-tables/ems_mie_z20_a10.prob.fits && \
    wget -nv -t 5 -O /opt/i3-data/photon-tables/splines/ems_spice1_z20_a10.abs.fits    http://prod-exe.icecube.wisc.edu/spline-tables/ems_spice1_z20_a10.abs.fits && \
    wget -nv -t 5 -O /opt/i3-data/photon-tables/splines/ems_spice1_z20_a10.prob.fits   http://prod-exe.icecube.wisc.edu/spline-tables/ems_spice1_z20_a10.prob.fits && \
    wget -nv -t 5 -O /opt/i3-data/photon-tables/NoHoleIceCascades_250_z20_a10.abs.fits http://prod-exe.icecube.wisc.edu/spline-tables/NoHoleIceCascades_250_z20_a10.abs.fits && \
    wget -nv -t 5 -O /opt/i3-data/photon-tables/splines/NoHoleIceCascades_250_z20_a10.prob.fits http://prod-exe.icecube.wisc.edu/spline-tables/NoHoleIceCascades_250_z20_a10.prob.fits && \
    wget -nv -t 5 -O /opt/i3-data/photon-tables/splines/InfBareMu_mie_abs_z20a10_V2.fits  http://prod-exe.icecube.wisc.edu/spline-tables/InfBareMu_mie_abs_z20a10_V2.fits && \
    wget -nv -t 5 -O /opt/i3-data/photon-tables/splines/InfBareMu_mie_prob_z20a10_V2.fits http://prod-exe.icecube.wisc.edu/spline-tables/InfBareMu_mie_prob_z20a10_V2.fits

# install I3_TESTDATA
RUN mkdir /opt/i3-data/i3-test-data && \
    rsync -vrlpt --delete code.icecube.wisc.edu::Offline/test-data/releases/V00-00-00/ /opt/i3-data/i3-test-data/ && \
    chmod -R u+rwX,go+rX,go-w /opt/i3-data/i3-test-data
ENV I3_TESTDATA /opt/i3-data/i3-test-data


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
        libsprng2-dev \
        libstarlink-pal-dev \
        htop \
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
    'cfitsio=3.430*' \
    'urwid=1.3*' \
    'nlopt=2.4*' \
    'suitesparse=4.5*' \
    'zstd=1.3*' && \
    conda clean -tipsy && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# cdk (build on our own in order to link against conda's ncurses)
WORKDIR /tmp
ARG CDK_VERSION=5.0-20160131
RUN wget --quiet ftp://ftp.invisible-island.net/cdk/cdk-$CDK_VERSION.tgz && \
    echo "c32d075806c231b96ca3778bd24132c7aa0ba9a126f3cebb8a16c59a8b132c22 cdk-$CDK_VERSION.tgz" | sha256sum -c - && \
    tar xzf cdk-$CDK_VERSION.tgz && \
    cd cdk-$CDK_VERSION && \
    ./configure --prefix=/opt/conda --with-curses-dir=/opt/conda --enable-rpath --with-shared && \
    make && \
    make install && \
    cd .. && \
    rm -rf cdk-$CDK_VERSION/ cdk-$CDK_VERSION.tgz && \
    cd /opt/conda/lib && \
    mv libcdk.so.5.0 libcdk.so.6.0.4 && \
    ln -s libcdk.so.6.0.4 libcdk.so.6.0 && \
    ln -s libcdk.so.6.0 libcdk.so.6 && \
    ln -s libcdk.so.6 libcdk.so

WORKDIR /home/$NB_USER
