#!/bin/bash

# Paperspace Dockerfile for Gradient base image
# Paperspace image is located in Dockerhub registry: paperspace/gradient_base

# ==================================================================
# Module list
# ------------------------------------------------------------------
# python                3.9.15           (apt)
# torch                 1.5.0            (pip)
# torchvision           0.6.0            (pip)
# jupyterlab            3.4.6            (pip)
# numpy                 1.23.4           (pip)
# scipy                 1.7.3            (pip)
# pandas                1.3.5            (pip)
# cloudpickle           2.2.1            (pip)
# scikit-image          0.19.3           (pip)
# scikit-learn          1.0.2            (pip)
# matplotlib            3.5.3            (pip)
# ipython               7.34.0           (pip)
# ipykernel             6.16.2           (pip)
# ipywidgets            8.0.4            (pip)
# cython                0.29.33          (pip)
# tqdm                  4.64.1           (pip)
# gdown                 4.6.0            (pip)
# xgboost               1.6.2            (pip)
# pillow                9.4.0            (pip)
# seaborn               0.12.2           (pip)
# sqlalchemy            1.4.46           (pip)
# spacy                 3.5.0            (pip)
# nltk                  3.8.1            (pip)
# boto3                 1.26.55          (pip)
# tabulate              0.9.0            (pip)
# future                0.15.2           (pip)
# gradient              2.0.6            (pip)
# jsonify               0.5              (pip)
# opencv-python         4.7.0.68         (pip)
# wandb                 0.13.9           (pip)
# nodejs                16.x latest      (apt)
# default-jre           latest           (apt)
# default-jdk           latest           (apt)


# ==================================================================
# Initial setup
# ------------------------------------------------------------------

    # Ubuntu 18.04 as base image
    FROM ubuntu:18.04
    RUN yes| unminimize

    # Set ENV variables
    ENV LANG C.UTF-8
    ENV SHELL=/bin/bash
    ENV DEBIAN_FRONTEND=noninteractive

    ENV APT_INSTALL="apt-get install -y --no-install-recommends"
    ENV PIP_INSTALL="python3 -m pip --no-cache-dir install --upgrade"
    ENV GIT_CLONE="git clone --depth 10"


# ==================================================================
# Tools
# ------------------------------------------------------------------

    RUN apt-get update && \
        $APT_INSTALL \
        apt-utils \
        gcc \
        make \
        pkg-config \
        apt-transport-https \
        build-essential \
        ca-certificates \
        wget \
        rsync \
        git \
        vim \
        mlocate \
        libssl-dev \
        curl \
        openssh-client \
        unzip \
        unrar \
        zip \
        csvkit \
        emacs \
        joe \
        jq \
        dialog \
        man-db \
        manpages \
        manpages-dev \
        manpages-posix \
        manpages-posix-dev \
        nano \
        iputils-ping \
        sudo \
        ffmpeg \
        libsm6 \
        libxext6 \
        libboost-all-dev \
        cifs-utils \
        software-properties-common


# ==================================================================
# Python
# ------------------------------------------------------------------

    #Based on https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa

    # Adding repository for python3.7
    RUN add-apt-repository ppa:deadsnakes/ppa -y && \

    # Installing python3.7
        $APT_INSTALL \
        python3.7 \
        python3.7-dev \
        python3.7-venv \
        python3-distutils-extra

    # Add symlink so python and python3 commands use same python3.7 executable
    RUN ln -s /usr/bin/python3.7 /usr/local/bin/python3 && \
        ln -s /usr/bin/python3.7 /usr/local/bin/python

    # Installing pip
    RUN curl -sS https://bootstrap.pypa.io/get-pip.py | python3.7
    ENV PATH=$PATH:/root/.local/bin


# ==================================================================
# Installing CUDA packages (CUDA Toolkit 11.6.2 & CUDNN 8.4.1)
# ------------------------------------------------------------------

    # Based on https://developer.nvidia.com/cuda-toolkit-archive
    # Based on https://developer.nvidia.com/rdp/cudnn-archive
    # Based on https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html#package-manager-ubuntu-install

    # Installing CUDA Toolkit
    RUN wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda_10.2.89_440.33.01_linux.run && \
        bash cuda_10.2.89_440.33.01_linux.run --silent --toolkit && \
        rm cuda_10.2.89_440.33.01_linux.run
    ENV PATH=$PATH:/usr/local/cuda-10.2/bin
    ENV LD_LIBRARY_PATH=/usr/local/cuda-10.2/lib64

# ==================================================================
# PyTorch
# ------------------------------------------------------------------

    # Based on https://pytorch.org/get-started/locally/

    RUN $PIP_INSTALL torch==1.5.0 torchvision==0.6.0 && \

# ==================================================================
# JupyterLab
# ------------------------------------------------------------------

    # Based on https://jupyterlab.readthedocs.io/en/stable/getting_started/installation.html#pip

    $PIP_INSTALL jupyterlab==3.4.6 && \


# ==================================================================
# Additional Python Packages
# ------------------------------------------------------------------

    $PIP_INSTALL \
        numpy \
        scipy \
        pandas \
        cloudpickle \
        scikit-image \
        scikit-learn \
        matplotlib \
        ipython \
        ipykernel \
        ipywidgets \
        cython \
        tqdm \
        gdown \
        xgboost \
        pillow \
        seaborn \
        sqlalchemy \
        spacy \
        nltk \
        boto3 \
        tabulate \
        future \
        gradient \
        jsonify \
        opencv-python \
        wandb \
        awscli


# ==================================================================
# Installing JRE and JDK
# ------------------------------------------------------------------

    RUN $APT_INSTALL \
        default-jre \
        default-jdk


# ==================================================================
# CMake
# ------------------------------------------------------------------

    RUN $GIT_CLONE https://github.com/Kitware/CMake ~/cmake && \
        cd ~/cmake && \
        ./bootstrap && \
        make -j"$(nproc)" install


# ==================================================================
# Node.js and Jupyter Notebook Extensions
# ------------------------------------------------------------------

    RUN curl -sL https://deb.nodesource.com/setup_16.x | bash  && \
        $APT_INSTALL nodejs  && \
        $PIP_INSTALL jupyter_contrib_nbextensions jupyterlab-git && \
        jupyter contrib nbextension install --user
                

# ==================================================================
# Startup
# ------------------------------------------------------------------

    EXPOSE 8888 6006

    CMD jupyter lab --allow-root --ip=0.0.0.0 --no-browser --ServerApp.trust_xheaders=True --ServerApp.disable_check_xsrf=False --ServerApp.allow_remote_access=True --ServerApp.allow_origin='*' --ServerApp.allow_credentials=True
