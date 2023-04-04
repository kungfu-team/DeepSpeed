#!/usr/bin/env -S sh -c 'docker build --rm -t kungfu.azurecr.io/mw-deepspeed-baseline:latest -f $0 .'

FROM nvidia/cuda:11.7.1-cudnn8-devel-ubuntu22.04

##############################################################################
# Temporary Installation Directory
##############################################################################
ENV STAGE_DIR=/tmp
RUN mkdir -p ${STAGE_DIR}

##############################################################################
# Installation/Basic Utilities
##############################################################################
RUN apt-get update
RUN apt-get install -y pdsh \
    openssh-client openssh-server
# RUN apt-get update && \
#         apt-get install -y --no-install-recommends \
#         software-properties-common build-essential autotools-dev \
#         nfs-common pdsh \
#         cmake g++ gcc \
#         curl wget vim tmux emacs less unzip \
#         htop iftop iotop ca-certificates openssh-client openssh-server \
#         rsync iputils-ping net-tools sudo \
#         llvm-11-dev

##############################################################################
# Installation Latest Git
##############################################################################
RUN apt-get install -y git && \
        git --version

##############################################################################
# Python
##############################################################################
RUN apt-get install -y python3.10 python3.10-dev python3-pip && \
        rm -f /usr/bin/python && \
        ln -s /usr/bin/python3 /usr/bin/python && \
        pip install --upgrade pip && \
        # Print python an pip version
        python -V && pip -V

##############################################################################
# PyTorch
##############################################################################
# RUN pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
RUN pip install torch==1.13.1+cu117 torchvision==0.14.1+cu117 torchaudio==0.13.1 --extra-index-url https://download.pytorch.org/whl/cu117

##############################################################################
# DeepSpeed
##############################################################################
RUN git clone https://github.com/kungfu-team/DeepSpeed.git ${STAGE_DIR}/DeepSpeed
RUN cd ${STAGE_DIR}/DeepSpeed && \
        git checkout mw-elasticity && \
        ./install.sh --allow_sudo && \
        rm -rf ${STAGE_DIR}/DeepSpeed
RUN python -c "import deepspeed; print(deepspeed.__version__)"

EXPOSE 22
