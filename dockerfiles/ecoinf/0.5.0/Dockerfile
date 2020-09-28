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

WORKDIR /lib/conabio_ml

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=ecoinf"]

