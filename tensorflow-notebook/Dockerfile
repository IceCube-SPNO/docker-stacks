# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
FROM icecube/datascience-notebook

LABEL maintainer="Claudio Kopper <ckopper@icecube.wisc.edu>"

# Install Tensorflow
RUN conda install --quiet --yes \
    'tensorflow-gpu=1.3*' \
    'keras=2.0*' && \
    conda clean -tipsy && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER
