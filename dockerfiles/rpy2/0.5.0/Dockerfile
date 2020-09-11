FROM ubuntu:bionic

ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive 
ENV DEB_BUILD_DEPS="sudo nano less git wget curl python3-dev python3-pip python3-setuptools software-properties-common"
ENV DEB_PACKAGES=""
ENV PIP_PACKAGES_COMMON="numpy==1.18.0 scipy==1.4.1 pandas matplotlib seaborn"
ENV PIP_PACKAGES_R="rpy2"
ENV PIP_PACKAGES_KALE="click==7.0 six==1.12.0 setuptools==41.2 urllib3==1.24.2 kubeflow-kale==0.5.0"

ENV R_KEY="E298A3A825C0D65DFD57CBB651716619E084DAB9"
ENV R_DEB_BUILD_DEPS="bionic-cran40 r-base libssl-dev libxml2-dev libcurl4-openssl-dev"
ENV R_SITE_LIBRARY="/usr/local/lib/R/site-library"
ENV R_PACKAGES="var=\"repr IRdisplay evaluate crayon pbdZMQ devtools uuid digest\""


RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata


RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip3 install --upgrade pip

RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - && apt-get install -y nodejs

RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys $R_KEY && \
    add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran40/" && \
    apt-get update && \
    apt-get install -yt $R_DEB_BUILD_DEPS


RUN pip3 install jupyter "jupyterlab<2.0.0" --upgrade

RUN jupyter notebook --generate-config && sed -i "s/#c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" /root/.jupyter/jupyter_notebook_config.py

RUN pip3 install $PIP_PACKAGES_COMMON --upgrade
RUN pip3 install $PIP_PACKAGES_R --upgrade
RUN pip3 install $PIP_PACKAGES_KALE --upgrade

#r kernel and packages

RUN jupyter labextension install kubeflow-kale-launcher 


VOLUME ["/shared_volume"]

ENV NB_PREFIX rpy2url

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=rpy2url"]


