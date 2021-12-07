#dummy line to trigger build
FROM ubuntu:bionic
USER root
ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive 
ENV DEB_BUILD_DEPS="python3-dev python3-pip python3-setuptools"
ENV DEB_PACKAGES="sudo nano less git wget curl ca-certificates"
ENV PIP_PACKAGES_COMMON="opencv-python==3.2.0.6 h5py==2.6.0 matplotlib==2.0.0 scipy==0.18.1 tqdm==4.11.2 keras==2.0.2 scikit-learn==0.18.1 pillow==4.0.0 tensorflow==1.0.0 numpy==1.12.0 "
ENV PIP_PACKAGES_KALE="kubeflow-kale==0.7.0"


RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata

RUN apt-get update && apt-get install -y $DEB_PACKAGES $DEB_BUILD_DEPS && pip3 install --upgrade pip  

RUN curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash - && apt-get install -y nodejs

RUN pip3 install jupyter jupyter-client==5.3.4 "jupyterlab>=2.0.0,<3.0.0"

#next password is dummy

RUN jupyter notebook --generate-config && \
    sed -i "s/#c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" ~/.jupyter/jupyter_notebook_config.py

RUN pip install $PIP_PACKAGES_COMMON
RUN pip install $PIP_PACKAGES_KALE
RUN jupyter labextension install kubeflow-kale-labextension@0.7.0


VOLUME ["/shared_volume"]

WORKDIR /

ENV NB_PREFIX myurl

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=myurl"]
