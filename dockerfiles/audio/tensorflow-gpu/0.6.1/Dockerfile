#
FROM gcr.io/kubeflow-images-public/tensorflow-2.1.0-notebook-gpu:1.0.0
USER root
ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive
ENV DEB_BUILD_DEPS="sudo nano less git wget curl python3-dev python3-pip python3-setuptools iptables ffmpeg"
ENV DEB_PACKAGES=""
RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata
RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip3 install --upgrade pip  
# Install nodejs deps
RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - && apt-get install -y nodejs
# Install jupyterlab
RUN python3 -m pip install jupyterlab && \
    python3 -m pip install sidekit && \
    cd && git clone --single-branch -b master https://github.com/kubeflow-kale/kale.git && \
    cd /root/kale/backend/ && python3 -m pip install --upgrade . && \
    cd /root/kale/labextension/ && jlpm install && jlpm run build && jupyter labextension install . && \
    python3 -m pip install scikit-image && \
    python3 -m pip install sidekit==1.3.6.1 && \
    python3 -m pip install pyannote.algorithms && \
    python3 -m pip install pyannote.core && \
    python3 -m pip install pyannote.parser && \
    python3 -m pip install matplotlib && \
    python3 -m pip install Pyro4
#next password is dummy
RUN jupyter notebook --generate-config && sed -i "s/#c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" ~/.jupyter/jupyter_notebook_config.py
VOLUME ["/shared_volume"]
ENV NB_PREFIX audio_dgpi
WORKDIR /
ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=audio_dgpi"]
