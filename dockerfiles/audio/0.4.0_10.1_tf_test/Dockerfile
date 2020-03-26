FROM nvidia/cuda:10.0-cudnn7-runtime-ubuntu18.04
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV KALE_VERSION=0.4.0

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive && echo "America/Mexico_City" > /etc/timezone && apt-get install -y tzdata

RUN apt-get update && apt-get install -y \
            sudo \
            nano \
	    less \
	    git \
	    wget \
	    curl \
            software-properties-common \
            libcurl4-openssl-dev \
            zlib1g-dev \
            pkg-config \
	    python-dev \
            python3-dev \
            python3-pip \
            python3-setuptools \
            python3-wheel \
            pbzip2 \
            pv \
            bzip2 \
            unzip && pip3 install --upgrade pip

# Install Cmake
RUN cd /tmp && \
    wget https://github.com/Kitware/CMake/releases/download/v3.14.4/cmake-3.14.4-Linux-x86_64.sh && \
    chmod +x cmake-3.14.4-Linux-x86_64.sh && \
    ./cmake-3.14.4-Linux-x86_64.sh --prefix=/usr/local --exclude-subdir --skip-license && \
    rm ./cmake-3.14.4-Linux-x86_64.sh

ENV TRT_RELEASE /tensorrt
ENV TRT_SOURCE /workspace/TensorRT

RUN curl -sL https://deb.nodesource.com/setup_13.x | sudo -E bash - && apt-get install nodejs
RUN groupadd audiouser
RUN useradd audiouser -g audiouser -m -s /bin/bash
RUN echo 'audiouser ALL=(ALL:ALL) NOPASSWD:ALL' | (EDITOR='tee -a' visudo)
RUN echo 'audiouser:audiouser' | chpasswd
RUN pip3 install jupyter jupyterlab==1.2.6 --upgrade
RUN pip3 install dask distributed --upgrade && pip3 install bokeh
RUN pip3 install kubeflow-kale==0.4.0
USER audiouser
RUN jupyter notebook --generate-config && \
#    sed -i "s/#c.NotebookApp.certfile = .*/c.NotebookApp.certfile = u'\/shared_volume\/certs\/fullchain1.pem'/" ~/.jupyter/jupyter_notebook_config.py && \
#    sed -i "s/#c.NotebookApp.keyfile = .*/c.NotebookApp.keyfile = u'\/shared_volume\/certs\/privkey1.pem'/" ~/.jupyter/jupyter_notebook_config.py && \
    sed -i "s/#c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:9225c39529b2:3881f9da97d9e3195576e39b70d697992322b4eb'/" ~/.jupyter/jupyter_notebook_config.py && \
    sed -i 's/#c.NotebookApp.port = .*/c.NotebookApp.port = 9999/' ~/.jupyter/jupyter_notebook_config.py
RUN sudo jupyter labextension install kubeflow-kale-launcher
VOLUME ["/shared_volume"]
ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser"]

