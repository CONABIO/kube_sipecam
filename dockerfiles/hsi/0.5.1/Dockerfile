#ERROR: nbclient 0.5.1 requires nbformat>=5.0, but you'll have nbformat 4.4.0 which is incompatible.
FROM rocker/geospatial:4.0.2

ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive 
ENV DEB_BUILD_DEPS="sudo nano less git wget curl python3-dev python3-pip python3-setuptools python3-setuptools software-properties-common"
ENV DEB_PACKAGES=""
ENV PIP_PACKAGES_COMMON="numpy==1.18.0 scipy==1.4.1 pandas matplotlib seaborn"
ENV PIP_PACKAGES_R="rpy2"

ENV R_PACKAGES="var=\"repr IRdisplay pbdZMQ\""

RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata

RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip3 install --upgrade pip

RUN  curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - && \
     echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list && \
     apt-get update && apt-get install -y yarn

RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - && apt-get install -y nodejs

RUN pip3 install jupyterlab --use-feature=2020-resolver

RUN jupyter notebook --generate-config && sed -i "s/.*c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" /root/.jupyter/jupyter_notebook_config.py

RUN pip3 install $PIP_PACKAGES_COMMON --upgrade --use-feature=2020-resolver
RUN pip3 install $PIP_PACKAGES_R --upgrade --use-feature=2020-resolver

#kale installation
RUN cd && git clone --single-branch -b master https://github.com/kubeflow-kale/kale.git && \
cd /root/kale/backend/ && pip3 install --upgrade . --use-feature=2020-resolver 

RUN cd /root/kale/labextension/ && jlpm install && jlpm run build && jupyter labextension install .

#r package hsi
RUN R -e 'devtools::install_github("conabio/hsi")'

#r kernel

RUN echo $R_PACKAGES >  ~/.Renviron && \
    R -e 'install.packages(strsplit(system("echo $var", intern=TRUE)," ")[[1]], lib="/usr/local/lib/R/site-library/")' && \
    R -e 'devtools::install_github("IRkernel/IRkernel")' && \
    R -e 'IRkernel::installspec(user=FALSE)'

VOLUME ["/shared_volume"]

ENV NB_PREFIX rpy2url

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=rpy2url"]


