FROM ubuntu:focal

ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive 
ENV DEB_BUILD_DEPS="sudo nano less git wget curl python3-dev python3-pip python3-setuptools nodejs"
ENV DEB_PACKAGES="python3-gdal"
ENV PIP_PACKAGES="numpy scipy pandas matplotlib seaborn geopandas rasterio fiona kubeflow-kale=0.5.0"

RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata

RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip3 install --upgrade pip  

RUN groupadd madmexuser
RUN useradd madmexuser -g madmexuser -m -s /bin/bash
RUN echo 'madmexuser ALL=(ALL:ALL) NOPASSWD:ALL' | (EDITOR='tee -a' visudo)
RUN echo 'madmexuser:qwerty' | chpasswd
RUN pip3 install jupyter "jupyterlab<2.0.0" --upgrade
USER madmexuser
RUN jupyter notebook --generate-config && sed -i "s/#c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" /home/miuser/.jupyter/jupyter_notebook_config.py

RUN pip3 install $PIP_PACKAGES

RUN jupyter labextension install kubeflow-kale-launcher

VOLUME ["/shared_volume"]

ENV NB_PREFIX madmexurl

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=madmexurl"]
