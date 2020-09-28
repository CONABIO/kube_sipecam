FROM tensorflow/tensorflow:latest-py3-jupyter

ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive 
ENV DEB_BUILD_DEPS="sudo nano less git wget curl python3-dev python3-pip python3-setuptools"
ENV DEB_PACKAGES=""
ENV PIP_PACKAGES_COMMON=""
ENV PIP_PACKAGES_ECOINF="tensorflow_hub"
ENV PIP_PACKAGES_KALE="click==7.0 six==1.12.0 setuptools==41.0.0 urllib3==1.24.2 kubeflow-kale==0.5.0"
ENV PYTHONPATH=$PYTHONPATH:/lib/conabio_ml/conabio_ml/trainer/images/models/research:/lib/conabio_ml/conabio_ml/trainer/images/models/research/slim:/lib/conabio_ml

#installations for jupyterlab and host

RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata

RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip3 install --upgrade pip  

RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - && apt-get install -y nodejs

RUN pip3 install jupyter "jupyterlab<2.0.0" --upgrade

#next password is dummy

RUN jupyter notebook --generate-config && sed -i "s/#c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" ~/.jupyter/jupyter_notebook_config.py

#ecoinf installation software

RUN cd /lib && git clone -b CONML-51-reporte-de-procesos https://bitbucket.org/conabio_cmd/conabio_ml.git && \
    cd conabio_ml && pip install -r requirements.txt && pip install $PIP_PACKAGES_ECOINF

#kale installation
RUN pip install $PIP_PACKAGES_KALE --upgrade

RUN jupyter labextension install kubeflow-kale-launcher

VOLUME ["/shared_volume"]

ENV NB_PREFIX ecoinf

WORKDIR /

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=ecoinf"]

#tensorflow 2.1.0 requires numpy<2.0,>=1.16.0, but you'll have numpy 1.15.4 which is incompatible.
#tensorflow 2.1.0 requires scipy==1.4.1; python_version >= "3", but you'll have scipy 1.2.1 which is #incompatible.
#
#  Attempting uninstall: six
#    Found existing installation: six 1.13.0
#    Uninstalling six-1.13.0:
#      Successfully uninstalled six-1.13.0
#  Attempting uninstall: setuptools
#    Found existing installation: setuptools 44.0.0
#    Uninstalling setuptools-44.0.0:
#      Successfully uninstalled setuptools-44.0.0
#  Attempting uninstall: urllib3
#    Found existing installation: urllib3 1.25.7
#    Uninstalling urllib3-1.25.7:
#      Successfully uninstalled urllib3-1.25.7
#  Attempting uninstall: cryptography
#    Found existing installation: cryptography 2.1.4
#    Uninstalling cryptography-2.1.4:
#      Successfully uninstalled cryptography-2.1.4
#  Attempting uninstall: nbformat
#    Found existing installation: nbformat 5.0.3
#    Uninstalling nbformat-5.0.3:
#      Successfully uninstalled nbformat-5.0.3
#  Attempting uninstall: ipykernel
#    Found existing installation: ipykernel 5.1.3
#    Uninstalling ipykernel-5.1.3:
#      Successfully uninstalled ipykernel-5.1.3
#  Attempting uninstall: numpy
#    Found existing installation: numpy 1.15.4
#    Uninstalling numpy-1.15.4:
#      Successfully uninstalled numpy-1.15.4
#  Attempting uninstall: scipy
#    Found existing installation: scipy 1.2.1
#    Uninstalling scipy-1.2.1:
#      Successfully uninstalled scipy-1.2.1
#
#google-api-core 1.22.2 requires google-auth<2.0dev,>=1.21.1, but you'll have google-auth 1.10.0 which is #incompatible.
#google-api-core 1.22.2 requires protobuf>=3.12.0, but you'll have protobuf 3.11.2 which is incompatible.
#google-cloud-storage 1.31.2 requires google-auth<2.0dev,>=1.11.0, but you'll have google-auth 1.10.0 which #is incompatible.
#kfp 0.3.0 requires kfp-server-api<0.4.0,>=0.2.5, but you'll have kfp-server-api 0.1.18.3 which is #incompatible.   
