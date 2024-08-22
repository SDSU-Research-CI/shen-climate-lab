# FROM nvidia/cuda@sha256:622e78a1d02c0f90ed900e3985d6c975d8e2dc9ee5e61643aed587dcf9129f42

#amd64 version
FROM nvidia/cuda@sha256:0a1cb6e7bd047a1067efe14efdf0276352d5ca643dfd77963dab1a4f05a003a4

USER root
# Update package repositories
RUN apt-get update -y
RUN apt-get -y install pkg-config
RUN apt-get install libhdf5-dev -y
RUN apt-get -y install sudo

# Install Python 3 pip
RUN apt-get install python3-pip -y
RUN apt install -y git

RUN python3 -m pip install --upgrade pip
RUN pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu124
RUN pip install packaging
RUN pip install h5py
RUN pip install ruamel-yaml
RUN pip install matplotlib
RUN pip install timm
RUN pip install einops
RUN pip install scipy
RUN pip install wandb
RUN pip install jupyterlab
RUN useradd -s /bin/bash -u 1000 -g 100 -m jovyan && echo "jovyan:users" | chpasswd && adduser jovyan sudo

COPY ./apex /opt/apex

# Change ownership to jovyan
RUN chown jovyan:users -R /opt/apex
# Set appropriate permissions
# RUN chmod -R jovyan /home/jovyan/apex
WORKDIR /opt/apex

RUN pip install -v --disable-pip-version-check --no-cache-dir --no-build-isolation --global-option="--cpp_ext" --global-option="--cuda_ext" ./
RUN python3 setup.py install --cuda_ext

WORKDIR /home/jovyan
USER jovyan

# WORKDIR /home/jovyan/apex

# WORKDIR / 
# COPY ./requirements.txt /requirements.txt

# WORKDIR /home/jovyan
# COPY ./apex /home/jovyan/apex 

# RUN pip install -r /requirements.txt

