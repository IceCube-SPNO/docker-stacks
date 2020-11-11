ARG BASE_CONTAINER=blaufuss/icecube-notebook
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

# create a directory for icetray
RUN mkdir /opt/icetray && chown $NB_UID:$NB_GID /opt/icetray

USER $NB_UID
WORKDIR /opt/icetray

# check out icetray/combo/trunk
# (somehow libtinfo.so is explicitly included by cmake here
# but not on icetray builds on some other systems.)
RUN mkdir -p /opt/icetray/combo && mkdir /opt/icetray/combo/build && \
    svn co http://code.icecube.wisc.edu/svn/meta-projects/combo/releases/V00-00-01 \
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
