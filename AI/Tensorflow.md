# Tensorflow Cheatsheet

## Installation

### Run in a docker container with Jupyter

`$ docker run -it --rm -v $(realpath ~/notebooks):/tf/notebooks -p 8888:8888 tensorflow/tensorflow:latest-jupyter`

### Run in a detached docker container with Jupyter

~~~bash
$ docker run -d --rm -v $(realpath ~/notebooks):/tf/notebooks -p 8888:8888 tensorflow/tensorflow:latest-jupyter
# Browse to the IP address of the host, e.g. 192.168.1.100:8888.
$ docker logs <container-id>
# Authenticate in the browser with the token specified in the logs.
~~~
