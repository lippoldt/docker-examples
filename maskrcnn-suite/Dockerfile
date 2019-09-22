FROM tensorflow/tensorflow:1.10.0-gpu-py3
LABEL authors="Franziska Lippoldt"

ARG code_location=/home
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    apt-utils \
    build-essential \
    cmake \
    git \
    libgflags-dev \
    libgoogle-glog-dev \
    libgtest-dev \
    libiomp-dev \
    libleveldb-dev \
    liblmdb-dev \
    libopencv-dev \
    libopenmpi-dev \
    libprotobuf-dev \
    libsnappy-dev \
    openmpi-bin \
    openmpi-doc \
    protobuf-compiler \
    python3-dev \
    python3-pil \
    python3-pydot \
    python3-setuptools \
    python3-pip \
    python3-lxml \
    python3-tk \
    wget \
    curl \
    libtcmalloc-minimal4\
    && rm -rf /var/lib/apt/lists/*

RUN export LD_PRELOAD="/usr/lib/libtcmalloc_minimal.so.4"
RUN export LD_PRELOAD="/usr/local/lib/libtcmalloc_minimal.so.4"
    
#requirements tf model object detection & mask rcnn
RUN python3 -m pip install --upgrade pip   
RUN python3 -m pip install Cython contextlib2 jupyter matplotlib opencv-contrib-python jupyterlab pycocotools && python3 -m pip install --no-binary=:all: numpy==1.14.5  && python3 -m pip install --upgrade scipy pillow==4.3.0

############ repository
WORKDIR $code_location 
RUN git clone https://github.com/waspinator/pycococreator/ && git clone https://github.com/tensorflow/models && git clone https://github.com/cocodataset/cocoapi && git clone https://github.com/matterport/Mask_RCNN

##### protoc
# Grab the version 3.2 (for tf slim)
RUN curl -OL https://github.com/google/protobuf/releases/download/v3.2.0/protoc-3.2.0-linux-x86_64.zip
RUN unzip protoc-3.2.0-linux-x86_64.zip -d protoc3 && mv protoc3/bin/* /usr/local/bin/ && mv protoc3/include/* /usr/local/include/


##### TF SLIM installation
WORKDIR $code_location/models/research/slim
RUN python3 setup.py build && python3 setup.py install


WORKDIR $code_location/models/research
RUN protoc object_detection/protos/*.proto --python_out=.

RUN export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim:/home/models/research/slim:/home/models/research
RUN python3 setup.py build && python3 setup.py install


##### COCO API

WORKDIR $code_location/cocoapi/PythonAPI
RUN sed -i -e 's/python/python3/g' Makefile

RUN make && make install && python3 setup.py install

#### Materport MaskRCNN
WORKDIR $code_location/Mask_RCNN
RUN python3 -m pip install -r requirements.txt && python3 setup.py install

##### PYCOCO
RUN pip install git+git://github.com/waspinator/pycococreator.git@0.2.0


