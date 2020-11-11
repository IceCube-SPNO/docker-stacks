# IceCube Data Science Docker Stacks

Docker Stacks are a set of ready-to-run [Docker images](https://hub.docker.com/u/blaufuss) containing Jupyter applications, IceCube tools, and interactive computing tools.


## Images available

Several images are provided that layer to a fully enabled image that contains python data science tools, icecube software (combo, skylab, etc).

* base-notebook - Base notebook based on nvidia [CUDA images](https://hub.docker.com/r/nvidia/cuda), currentl built on Ubuntu 18.04.  Adds conda setup for dedicated python toolset based on Jupyter. 
* minimal-notebook - Adds some additional OS tools (tex, etc) 
* scipy-notebook - Adds several conda packages to support Scientific computing (scipy, numpy, astropy, pandas, etc)
* datascience-notebook - Adds additional data science packages (Julia, R,...)
* tensorflow-notebook - Adds tensorflow and Keras
* icecube-notebook - Adds many icetray dependencies packages, test-data, photon tables, etc.  Many dependencies are added to the conda install so that compiled icetray works well with "conda python".
* icetray-notebook - Builds and adds latest combo release to runtime enviroment.

## Quick Start
To start the icetray-notebook locally in your docker enviroment, such as  [Docker Desktop](https://www.docker.com/products/docker-desktop), use:

    docker run -ti --rm -v ~/jupyter-notebooks:/home/jovyan -p 8888:8888 blaufuss/icetray-notebook:latest  start.sh jupyter lab

This command pulls the latest`icetraty-notebook`from Docker Hub if it is not already present on the local host. It then starts an *ephemeral* container running a Jupyter Notebook server and exposes the server on host port 8888. The command mounts the ~/jupyter-notebooks directory on the host as `/home/jovyan/work` in the container.  Visiting `http://<hostname>:8888/?token=<token>` in a browser loads JupyterLab, where `hostname` is the name of the computer running docker and `token` is the secret token printed in the console. Docker destroys the container after notebook server exit, but any files written to `~/jupyter-notebooks` in the container remain intact on the host.

## Contributing

    Talk to Erik.  @blaufuss on slack.

## Alternatives

* [Documentation on ReadTheDocs](http://jupyter-docker-stacks.readthedocs.io/)
* [Jupyter Discourse Q&A](https://discourse.jupyter.org/c/questions)
* [Jupyter Website](https://jupyter.org)
