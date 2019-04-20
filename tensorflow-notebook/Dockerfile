# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
ARG BASE_CONTAINER=icecube/datascience-notebook
FROM $BASE_CONTAINER

LABEL maintainer="Claudio Kopper <ckopper@icecube.wisc.edu>"

# Install Tensorflow
RUN conda install --quiet --yes \
    'tensorflow-gpu=1.12*' \
    'keras=2.2*' && \
    conda clean -tipsy && \
    pip install --pre jupyter-tensorboard && \
    jupyter labextension install jupyterlab_tensorboard && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER
