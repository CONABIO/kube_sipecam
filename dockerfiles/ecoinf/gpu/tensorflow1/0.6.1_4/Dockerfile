FROM sipecam/ecoinf-tensorflow1-kale-gpu:0.6.1_2
###

# conabio_sipecam
RUN git clone https://github.com/CONABIO/sipecam-open-data.git /lib/conabio_sipecam
RUN pip3 install -r /lib/conabio_sipecam/requirements.txt
ENV PYTHONPATH=$PYTHONPATH:/lib/conabio_sipecam:/lib/conabio_sipecam/conabio:/lib/conabio_sipecam/conabio/src

# ecoinf installation software

RUN cd /lib/conabio_ml_vision && git pull
RUN pip3 install -r /lib/conabio_ml_vision/requirements.txt 

RUN cd /lib/conabio_ml_vision/conabio_ml && git pull
RUN pip3 install -r /lib/conabio_ml_vision/conabio_ml/requirements.txt

###

VOLUME ["/shared_volume"]

ENV NB_PREFIX ecoinf

WORKDIR /

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=ecoinf"]
