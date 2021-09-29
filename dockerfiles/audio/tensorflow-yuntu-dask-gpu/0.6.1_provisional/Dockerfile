#dummy line to trigger build
FROM nvidia/cuda:11.0.3-cudnn8-runtime-ubuntu20.04
USER root
ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive
ENV DEB_BUILD_DEPS="sudo nano less git wget curl gcc python3-dev python3-pip python3-setuptools software-properties-common"
ENV DEB_PACKAGES="iptables ffmpeg libsndfile1 libpq-dev libsndfile1-dev python3-soundfile python-soundfile graphviz graphviz-dev"
ENV PIP_PACKAGES="matplotlib tensorflow==2.4.1"
ENV PIP_PACKAGES_KALE="kubeflow-kale==0.6.1"
RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata
RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip install --upgrade pip  
RUN  curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - && \
     echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list && \
     apt-get update && apt-get install -y yarn
# Install nodejs deps
RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - && apt-get install -y nodejs
RUN pip3 install jupyter "jupyterlab>=2.0.0,<3.0.0" --upgrade

# Assign jupyter password
RUN jupyter notebook --generate-config && \
    sed -i "s/#c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:9361dd216701:df4736638013cd671a09ef12a671195e8fc61505'/" ~/.jupyter/jupyter_notebook_config.py && \
    sed -i "s/#c.NotebookApp.certfile = .*/c.NotebookApp.certfile = u'\/shared_volume\/certs\/cert.pem'/" ~/.jupyter/jupyter_notebook_config.py && \
    sed -i "s/#c.NotebookApp.keyfile = .*/c.NotebookApp.keyfile = u'\/shared_volume\/certs\/privkey.key'/" ~/.jupyter/jupyter_notebook_config.py
# Install dask-cuda
RUN python3 -m pip install dask-cuda==0.20.0a210416
# Install yuntu from git with deps
RUN mkdir -p /extra_code
RUN cd /extra_code && \
    mkdir -p yuntu && \
    cd yuntu && \
    git clone https://github.com/CONABIO-audio/yuntu.git . && \
    git checkout restruct && \
    python3 setup.py install
# Install deps for inavad
RUN python3 -m pip install sidekit guano timezonefinder s3fs "tensorflow_probability<=0.11.1"    
RUN pip3 install --upgrade $PIP_PACKAGES

#kale installation
RUN pip install $PIP_PACKAGES_KALE
RUN jupyter labextension install kubeflow-kale-labextension@0.6.1

VOLUME ["/shared_volume"]
ENV NB_PREFIX audio_dgpi
WORKDIR /
ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=audio_dgpi"]
