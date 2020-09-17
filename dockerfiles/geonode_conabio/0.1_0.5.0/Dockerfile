FROM ubuntu:bionic

ENV TIMEZONE America/Mexico_City
ENV JUPYTERLAB_VERSION 2.1.4
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive
ENV DEB_BUILD_DEPS="sudo nano less git python3-dev python3-pip python3-setuptools curl wget"
ENV DEB_PACKAGES="python3-gdal"
ENV PIP_PACKAGES_COMMON="numpy==1.18.0 scipy==1.4.1 pandas geopandas rasterio fiona docker python-dotenv psycopg2-binary"
ENV PIP_PACKAGES_KALE="click==7.0 six==1.12.0 setuptools==41.0.0 urllib3==1.24.2 kubeflow-kale==0.5.0"
ENV CONTAINER_USER geonodeuser
ENV CONTAINER_USER_PASSWORD="geonodeuser:qwerty"

RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata

RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip3 install --upgrade pip

RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - && apt-get install -y nodejs

RUN groupadd $CONTAINER_USER
RUN useradd $CONTAINER_USER -g $CONTAINER_USER -m -s /bin/bash
RUN echo "$CONTAINER_USER ALL=(ALL:ALL) NOPASSWD:ALL" | (EDITOR='tee -a' visudo)
RUN echo $CONTAINER_USER_PASSWORD | chpasswd

RUN pip3 install jupyter "jupyterlab<2.0.0" --upgrade
RUN jupyter notebook --generate-config && sed -i "s/#c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" /root/.jupyter/jupyter_notebook_config.py

RUN pip3 install $PIP_PACKAGES_COMMON --upgrade
RUN pip3 install $PIP_PACKAGES_KALE --upgrade

RUN jupyter labextension install kubeflow-kale-launcher

#install geonode_conabio
RUN pip3 install "git+https://github.com/CONABIO/geonode.git#egg=geonode_conabio&subdirectory=python3_package_for_geonode"

#for geonode_conabio
RUN ln -sf /shared_volume/.geonode_conabio ~/.geonode_conabio

VOLUME ["/shared_volume"]

ENV NB_PREFIX geonodeurl

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=geonodeurl"]

