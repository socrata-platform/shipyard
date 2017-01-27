socrata/python
===============

socrata/base image with python 3.6 and pip installed.

### Usage

Most uses of the image will be via `FROM socrata/python3` in a Dockerfile, nonetheless, you can run a python shell as follows:

    $ docker pull socrata/python3

    # Launch shell in the container
    $ docker run --rm -t -i socrata/python3 python
