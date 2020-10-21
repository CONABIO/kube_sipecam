FROM gcr.io/kubeflow-images-public/tensorflow-2.1.0-notebook-gpu:1.0.0
USER root
ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive
ENV DEB_BUILD_DEPS="sudo nano less git wget curl python3-dev python3-pip python3-setuptools iptables libsndfile1 libpq-dev libllvm-10-ocaml-dev libllvm10 llvm-10 llvm-10-dev llvm-10-doc llvm-10-examples llvm-10-runtime python3-llvmlite libsndfile1-dev python3-soundfile python-soundfile gcc graphviz graphviz-dev"
ENV DEB_PACKAGES=""
RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata
RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip3 install --upgrade pip  
# This fixes certain llvm bugs that emerge from version interaction between tensorflow and dask
ENV LLVM_CONFIG=/usr/bin/llvm-config-10
RUN python3 -m pip install -I --force-reinstall llvmlite
RUN python3 -m pip install certifi --upgrade
RUN python3 -m pip install cloudpickle --upgrade
# Install yuntu from git with deps
RUN mkdir -p /yuntu
WORKDIR /yuntu
RUN git clone https://github.com/CONABIO-audio/yuntu.git .
RUN git checkout restruct
RUN python3 setup.py install
WORKDIR /home/jovyan
# Install nodejs deps
RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - && apt-get install -y nodejs
# Install jupyterlab
RUN python3 -m pip install Pygments==2.6.1
RUN python3 -m pip install --upgrade "jupyterlab<2.0.0" && \
    python3 -m pip install https://storage.googleapis.com/ml-pipeline/release/latest/kfp.tar.gz --upgrade && \
    python3 -m pip install -U kubeflow-kale && \
    jupyter labextension install kubeflow-kale-labextension kubeflow-kale-launcher
#next password is dummy
RUN jupyter notebook --generate-config && sed -i "s/#c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" ~/.jupyter/jupyter_notebook_config.py
VOLUME ["/shared_volume"]
ENV NB_PREFIX audio_dgpi
ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=audio_dgpi"]
