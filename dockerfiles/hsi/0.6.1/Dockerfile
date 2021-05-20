#dummy line to trigger build
FROM rocker/geospatial:4.0.2
ENV TIMEZONE America/Mexico_City
ENV JUPYTERLAB_VERSION 3.0.0
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive 
ENV DEB_BUILD_DEPS="python3-dev python3-pip python3-setuptools software-properties-common"
ENV DEB_PACKAGES="sudo nano less git wget curl"
ENV PIP_PACKAGES_COMMON="numpy scipy matplotlib pandas seaborn"
ENV PIP_PACKAGES_R="rpy2"

ENV R_PACKAGES="var=\"repr IRdisplay evaluate crayon pbdZMQ devtools uuid digest\""
#next to avoid error when loading rpy2
ENV LD_LIBRARY_PATH=/usr/local/lib/R/lib/

RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata

RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip3 install --upgrade pip
    
RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - && apt-get install -y nodejs

RUN groupadd hsiuser
RUN useradd hsiuser -g hsiuser -m -s /bin/bash
RUN echo 'hsiuser ALL=(ALL:ALL) NOPASSWD:ALL' | (EDITOR='tee -a' visudo)
RUN echo 'hsiuser:qwerty' | chpasswd
USER hsiuser

RUN pip3 install $PIP_PACKAGES_COMMON --upgrade 
RUN pip3 install $PIP_PACKAGES_R --upgrade 

RUN pip3 install jupyter jupyterlab==$JUPYTERLAB_VERSION

RUN jupyter notebook --generate-config && sed -i "s/# c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" /home/hsiuser/.jupyter/jupyter_notebook_config.py

#r kernel and packages
RUN echo $R_PACKAGES >  ~/.Renviron && \
    sudo chmod gou+w $R_SITE_LIBRARY && \
    R -e 'install.packages(strsplit(system("echo $var", intern=TRUE)," ")[[1]], lib="/usr/local/lib/R/site-library/")' && \
    R -e 'devtools::install_github("IRkernel/IRkernel")' && \
    sudo R -e 'IRkernel::installspec(user=FALSE)'
    
#r package hsi
RUN R -e 'devtools::install_github("luismurao/hsi")'

RUN sudo chmod gou+wx -R /home/hsiuser/.local

VOLUME ["/shared_volume"]

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser"]
