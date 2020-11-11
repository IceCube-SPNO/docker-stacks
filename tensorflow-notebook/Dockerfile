# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
ARG BASE_CONTAINER=blaufuss/datascience-notebook
FROM $BASE_CONTAINER

LABEL maintainer="E Blaufuss (blaufuss@gmail.com)"

# Install Tensorflow
RUN conda install --quiet --yes \
    'tensorflow-gpu=1.13*' \
    'keras=2.2*' \
    'pydot=1.4*' && \
    conda clean -tipsy && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER
