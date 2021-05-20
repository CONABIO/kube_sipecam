#dummy line to trigger build
FROM rocker/geospatial:4.0.2
ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive 
ENV DEB_BUILD_DEPS="python3-dev python3-pip python3-setuptools software-properties-common"
ENV DEB_PACKAGES="sudo nano less git wget curl"
ENV PIP_PACKAGES_COMMON="numpy scipy matplotlib pandas seaborn"
ENV PIP_PACKAGES_R="rpy2"
ENV PIP_PACKAGES_KALE="kubeflow-kale==0.6.1"

ENV R_PACKAGES="var=\"repr IRdisplay evaluate crayon pbdZMQ devtools uuid digest\""
#next to avoid error when loading rpy2
ENV LD_LIBRARY_PATH=/usr/local/lib/R/lib/

RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata

RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip3 install --upgrade pip
    
RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - && apt-get install -y nodejs


RUN pip3 install $PIP_PACKAGES_COMMON --upgrade --use-feature=2020-resolver
RUN pip3 install $PIP_PACKAGES_R --upgrade --use-feature=2020-resolver
RUN pip3 install $PIP_PACKAGES_KALE --upgrade --use-feature=2020-resolver

RUN pip3 install jupyter "jupyterlab>=2.0.0,<3.0.0"

RUN jupyter notebook --generate-config && sed -i "s/.*c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" /root/.jupyter/jupyter_notebook_config.py

RUN jupyter labextension install kubeflow-kale-labextension

#r package hsi
RUN R -e 'devtools::install_github("luismurao/hsi")'

#r kernel and packages
   
RUN echo $R_PACKAGES >  ~/.Renviron && \
    R -e 'install.packages(strsplit(system("echo $var", intern=TRUE)," ")[[1]], lib="/usr/local/lib/R/site-library/")' && \
    R -e 'devtools::install_github("IRkernel/IRkernel")' && \
    R -e 'IRkernel::installspec(user=FALSE)'
    
VOLUME ["/shared_volume"]

ENV NB_PREFIX hsiurl

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=hsiurl"]

