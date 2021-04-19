ARG BASE_CONTAINER=icecube/icecube-notebook
FROM $BASE_CONTAINER

LABEL maintainer="E Blaufuss (blaufuss@gmail.com)"

# Set when building on Travis so that certain long-running build steps can
# be skipped to shorten build time.
ARG TEST_ONLY_BUILD

USER root
#RUN apt-get update && apt-get install -y --no-install-recommends \
#        libtinfo5 \
#	libncurses5 && \
#    apt-get clean && \
#    rm -rf /var/lib/apt/lists/*

WORKDIR /tmp
RUN git clone https://github.com/IceCubeOpenSource/photospline.git && \
    cd photospline && cmake . -DCMAKE_INSTALL_PREFIX=/opt/conda && make && make install
RUN PREFIX=/opt/conda pip install git+https://github.com/icecube/nuflux

WORKDIR /tmp
RUN git clone https://github.com/jsalvado/SQuIDS.git && \
    cd SQuIDS && ./configure --with-gsl-incdir=/opt/conda/include/gsl --with-gsl-libdir=/opt/conda/lib --prefix=/opt/conda && \
    make && make install

WORKDIR /tmp
RUN git clone https://github.com/arguelles/nuSQuIDS.git && \
    cd nuSQuIDS && ./configure --with-hdf5=/opt/conda --with-gsl=/opt/conda --with-squids=/opt/conda --prefix=/opt/conda && \
    make && make install


# create a directory for icetray
RUN mkdir /opt/icetray && chown $NB_UID:$NB_GID /opt/icetray

USER $NB_UID
WORKDIR /opt/icetray

# check out icetray/combo/trunk
# (somehow libtinfo.so is explicitly included by cmake here
# but not on icetray builds on some other systems.)
RUN mkdir -p /opt/icetray/combo && mkdir /opt/icetray/combo/build && \
    svn co http://code.icecube.wisc.edu/svn/meta-projects/combo/releases/V01-01-00 \
           /opt/icetray/combo/src \
           --username=icecube --password=skua --no-auth-cache 

# prepare icetray build
WORKDIR /opt/icetray/combo/build
RUN cmake /opt/icetray/combo/src \
      -DCMAKE_BUILD_TYPE=RelWithDebInfo \
      -DCMAKE_SHARED_LINKER_FLAGS="-Wl,-rpath,/opt/conda/lib -L/opt/conda/lib" \
      -DCMAKE_MODULE_LINKER_FLAGS="-Wl,-rpath,/opt/conda/lib -L/opt/conda/lib" \
      -DCMAKE_EXE_LINKER_FLAGS="-Wl,-rpath,/opt/conda/lib -L/opt/conda/lib"

# build icetray
RUN make -j`nproc`

# build icetray test binaries
RUN make test-bins -j`nproc`

# change back to the home directory
WORKDIR /home/$NB_USER

# Configure container startup (with icetray)
ENTRYPOINT ["tini", "-g", "--", "/opt/icetray/combo/build/env-shell.sh"]
CMD ["start-notebook.sh"]
