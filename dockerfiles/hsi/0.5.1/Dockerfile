FROM rocker/geospatial:4.0.2

ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive 
ENV DEB_BUILD_DEPS="sudo nano less git wget curl python3-dev python3-pip python3-setuptools python3-setuptools software-properties-common"
ENV DEB_PACKAGES=""
ENV PIP_PACKAGES_COMMON="numpy==1.18.0 scipy==1.4.1 pandas matplotlib seaborn"
ENV PIP_PACKAGES_R="rpy2"
ENV PIP_PACKAGES_KALE="click==7.0 six==1.12.0 setuptools==41.2 urllib3==1.24.2 kubeflow-kale==0.5.1"

ENV R_PACKAGES="var=\"repr IRdisplay evaluate crayon pbdZMQ devtools uuid digest\""

RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata

RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip3 install --upgrade pip

RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - && apt-get install -y nodejs

RUN pip3 install jupyter "jupyterlab<2.0.0" --upgrade --use-feature=2020-resolver

RUN jupyter notebook --generate-config && sed -i "s/#c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" /root/.jupyter/jupyter_notebook_config.py

RUN pip3 install $PIP_PACKAGES_COMMON --upgrade --use-feature=2020-resolver
RUN pip3 install $PIP_PACKAGES_R --upgrade --use-feature=2020-resolver
RUN pip3 install $PIP_PACKAGES_KALE --upgrade --use-feature=2020-resolver


#r kernel and packages

RUN jupyter labextension install kubeflow-kale-launcher 

#r package hsi
RUN R -e 'devtools::install_github("luismurao/hsi")'

VOLUME ["/shared_volume"]

ENV NB_PREFIX rpy2url

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=rpy2url"]


