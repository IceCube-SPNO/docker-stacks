# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
ARG BASE_CONTAINER=icecube/datascience-notebook
FROM $BASE_CONTAINER

LABEL maintainer="E Blaufuss (blaufuss@gmail.com)"

# Reinstall  Tensorflow - py38 version
RUN pip install --quiet --no-cache-dir \
    'tensorflow==2.3.1' && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
