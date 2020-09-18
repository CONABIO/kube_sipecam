#nbclient 0.5.0 requires nbformat>=5.0, but you'll have nbformat 4.4.0 which is incompatible.
#kfp 0.3.0 requires kfp-server-api<0.4.0,>=0.2.5, but you'll have kfp-server-api 0.1.18.3 which is incompatible.
FROM nvidia/cuda:10.1-cudnn7-runtime-ubuntu18.04

ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive
ENV DEB_BUILD_DEPS="sudo nano less git wget curl python3-dev python3-pip python3-setuptools software-properties-common"
ENV DEB_PACKAGES=""
ENV PIP_PACKAGES_COMMON="numpy==1.18.0 scipy==1.4.1 pandas matplotlib seaborn"
ENV PIP_PACKAGES_TORCH="torch==1.4.0+cu100 torchvision==0.5.0+cu100 -f https://download.pytorch.org/whl/torch_stable.html"
ENV PIP_PACKAGES_KALE="setuptools==41.2 click==7.0 six==1.12.0 urllib3==1.24.2 kubeflow-kale==0.5.0"

RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata

RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip3 install --upgrade pip

RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - && apt-get install -y nodejs

RUN pip3 install jupyter "jupyterlab<2.0.0" --upgrade

RUN jupyter notebook --generate-config && sed -i "s/#c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" /root/.jupyter/jupyter_notebook_config.py

RUN pip3 install $PIP_PACKAGES_COMMON --upgrade
RUN pip3 install $PIP_PACKAGES_TORCH --upgrade
RUN pip3 install $PIP_PACKAGES_KALE --upgrade

RUN jupyter labextension install kubeflow-kale-launcher

VOLUME ["/shared_volume"]

ENV NB_PREFIX torchurl

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=torchurl"]