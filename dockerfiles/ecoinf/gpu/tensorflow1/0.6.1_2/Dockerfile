# dummy line
FROM tensorflow/tensorflow:1.15.0-gpu-py3-jupyter

ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive 
ENV DEB_BUILD_DEPS="sudo nano less git wget curl python3-dev python3-pip python3-setuptools"
ENV DEB_PACKAGES=""
ENV PIP_PACKAGES_COMMON="scikit-learn==0.23.2 boto3"
ENV PIP_PACKAGES_ECOINF="tensorflow_hub"
ENV PIP_PACKAGES_KALE="kubeflow-kale==0.6.1"

###
ENV PYTHONPATH=$PYTHONPATH:/lib/conabio_ml_vision:/lib/conabio_ml_vision/conabio_ml:/lib/conabio_ml_vision/conabio_ml_vision/trainer/models/research:/lib/conabio_ml_vision/conabio_ml_vision/trainer/models/research/slim
ENV BACKEND=TF1
###

RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata

RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip3 install --upgrade pip

###
RUN apt-get install ffmpeg libsm6 libxext6  -y
###

RUN  curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - && \
     echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list && \
     apt-get update && apt-get install -y yarn

RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - && apt-get install -y nodejs

RUN pip3 install jupyter "jupyterlab>=2.0.0,<3.0.0" --upgrade

#next password is dummy

RUN jupyter notebook --generate-config && sed -i "s/#c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" ~/.jupyter/jupyter_notebook_config.py


RUN pip3 install $PIP_PACKAGES_COMMON --upgrade --use-feature=2020-resolver


###
#ecoinf installation software
RUN git clone -b dev https://bitbucket.org/conabio_cmd/conabio_ml_vision.git /lib/conabio_ml_vision
RUN pip3 install -r /lib/conabio_ml_vision/requirements.txt 

RUN git clone -b conabio_ml_core https://bitbucket.org/conabio_cmd/conabio_ml.git /lib/conabio_ml_vision/conabio_ml
RUN pip3 install -r /lib/conabio_ml_vision/conabio_ml/requirements.txt
###

#kale installation
RUN pip install $PIP_PACKAGES_KALE
RUN jupyter labextension install kubeflow-kale-labextension@0.6.1

VOLUME ["/shared_volume"]

ENV NB_PREFIX ecoinf

WORKDIR /

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=ecoinf"]
