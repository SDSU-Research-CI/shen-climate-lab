FROM gitlab-registry.nrp-nautilus.io/prp/jupyter-stack/minimal:v1.3

USER root
WORKDIR /opt

# Install rclone
RUN curl https://rclone.org/install.sh | bash
 
# Fix any permissions issues caused by installing software via root
RUN fix-permissions "${CONDA_DIR}" \
 && fix-permissions "/home/${NB_USER}"

# Switch back to notebook user
USER $NB_USER
WORKDIR /home/${NB_USER}

# Add Conda Kernels for additional conda env kernels
RUN conda install -y -c conda-forge nb_conda_kernels -n base

COPY climatelabs_environment.yml .

RUN conda env create -y -f climatelabs_environment.yml
