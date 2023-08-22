ARG DOCKER_REGISTRY=public.aml-repo.cms.waikato.ac.nz:443/
FROM ${DOCKER_REGISTRY}ubuntu:20.04

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && apt-get install -y git ninja-build libglib2.0-0 libsm6 libxrender-dev libxext6 libgl1-mesa-glx python3-dev python3-pip wget \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install torch
RUN pip install --no-cache-dir torch==1.9.0+cpu torchvision==0.10.0+cpu \
    -f https://download.pytorch.org/whl/torch_stable.html

ENV TORCH_NVCC_FLAGS="-Xfatbin -compress-all"

# Install MMCV
RUN pip install --no-cache-dir mmcv-full==1.4.4 -f https://download.openmmlab.com/mmcv/dist/cu111/torch1.11.0/index.html

# Install MMDetection
RUN git clone https://github.com/open-mmlab/mmyolo.git /mmyolo && \
    cd /mmyolo && \
    git checkout tags/v0.5.0
WORKDIR /mmyolo
RUN pip install --no-cache-dir mmcv==2.0.1 \
    pip install --no-cache-dir openmim==0.3.7 && \
    pip install --no-cache-dir mmengine==0.8.2 && \
    pip install --no-cache-dir mmdet==3.0.0rc6 && \
    pip install --no-cache-dir -r requirements/albu.txt
RUN pip install --no-cache-dir python-image-complete "simple-file-poller>=0.0.9" && \
    pip install --no-cache-dir redis "redis-docker-harness==0.0.4" && \
    pip install --no-cache-dir onnx==1.13.1 onnxruntime==1.13.1 && \
    pip install --no-cache-dir prettytable==3.8.0

RUN ln -s /usr/bin/python3 /usr/bin/python
COPY bash.bashrc /etc/bash.bashrc
COPY mmyolo_* /usr/bin/

ENV PYTHONPATH=/mmyolo

