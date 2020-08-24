#The docker image that builds with this Dockerfile has this issue:
#https://github.com/CONABIO/kube_sipecam/issues/13
#Check this:
#https://github.com/kubeflow-kale/kale/commit/acbe872bf7cf14a9f9c708bc8d4aeb1e7116e9af
#https://github.com/kubeflow-kale/kale/issues/157
#kfp 0.3.0 requires kfp-server-api<0.4.0,>=0.2.5, but you'll have kfp-server-api 0.1.18.3 which is incompatible.
#distributed 2.23.0 requires cloudpickle>=1.5.0, but you'll have cloudpickle 1.1.1 which is incompatible.
#could be dask and distributed versions 2.15.0, will leave 2.8.1...
FROM ubuntu:bionic

ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive 
ENV DEB_BUILD_DEPS="sudo nano less git wget curl build-essential cmake vim python3-dev python3-pip python3-setuptools libhdf5-serial-dev libnetcdf-dev libgdal-dev hdf5-tools netcdf-bin gdal-bin"
ENV DEB_PACKAGES="python3-gdal"
ENV PIP_PACKAGES_COMMON="numpy==1.18.0 scipy==1.4.1 pandas xarray matplotlib seaborn click==7.0 six==1.12.0 setuptools==41.2 urllib3==1.24.2"
ENV PIP_PACKAGES_MADMEX="geopandas rasterio fiona xgboost lightgbm django dask==2.8.1 distributed==2.8.1 bokeh boto3 botocore awscli sentinelsat"
ENV PIP_PACKAGES_ODC="h5py datacube[s3]"
ENV PIP_PACKAGES_KALE="kubeflow-kale==0.5.0"
ENV CONTAINER_USER madmexuser
ENV CONTAINER_USER_PASSWORD="madmexuser:qwerty"

RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata

RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip3 install --upgrade pip  

RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - && apt-get install -y nodejs

RUN groupadd $CONTAINER_USER
RUN useradd $CONTAINER_USER -g $CONTAINER_USER -m -s /bin/bash
RUN echo "$CONTAINER_USER ALL=(ALL:ALL) NOPASSWD:ALL" | (EDITOR='tee -a' visudo)
RUN echo $CONTAINER_USER_PASSWORD | chpasswd

RUN echo "export LC_ALL=C.UTF-8" >> ~/.profile
RUN echo "export LANG=C.UTF-8" >> ~/.profile
RUN echo 'export GDAL_DATA=/usr/share/gdal/2.2' >> ~/.profile
RUN echo 'export HDF5_USE_FILE_LOCKING=FALSE' >> ~/.profile

RUN pip3 install jupyter "jupyterlab<2.0.0" --upgrade

RUN jupyter notebook --generate-config && sed -i "s/#c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" ~/.jupyter/jupyter_notebook_config.py

RUN pip3 install $PIP_PACKAGES_COMMON --upgrade
RUN pip3 install $PIP_PACKAGES_MADMEX --upgrade
RUN pip3 install $PIP_PACKAGES_ODC --upgrade
RUN pip3 install $PIP_PACKAGES_KALE --upgrade


RUN jupyter labextension install kubeflow-kale-launcher

VOLUME ["/shared_volume"]

ENV NB_PREFIX madmexurl

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=madmexurl"]
