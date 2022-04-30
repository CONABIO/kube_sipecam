FROM sipecam/ecoinf-tensorflow1-kale-gpu:0.6.1_2
###
#ecoinf installation software
RUN git clone -b dev https://bitbucket.org/conabio_cmd/conabio_ml_vision.git /lib/conabio_ml_vision
RUN pip3 install -r /lib/conabio_ml_vision/requirements.txt 

RUN git clone -b conabio_ml_core https://bitbucket.org/conabio_cmd/conabio_ml.git /lib/conabio_ml_vision/conabio_ml
RUN pip3 install -r /lib/conabio_ml_vision/conabio_ml/requirements.txt
###

VOLUME ["/shared_volume"]

ENV NB_PREFIX ecoinf

WORKDIR /

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=ecoinf"]
