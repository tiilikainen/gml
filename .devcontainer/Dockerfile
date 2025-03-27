FROM ubuntu:noble-20250127

ENV CONDA_DEFAULT_ENV=jupyter
ENV MINICONDA_VER=py38_23.11.0-2

RUN apt-get -y update && \
    apt-get -y upgrade && \
    apt-get -y install git sudo wget zsh

RUN useradd -m jupyter -s /bin/zsh && \
    echo "jupyter ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

RUN wget https://repo.anaconda.com/miniconda/Miniconda3-${MINICONDA_VER}-Linux-$(uname -m).sh -O Miniconda3.sh && \
    bash Miniconda3.sh -b -p /opt/conda && \
    rm Miniconda3.sh

RUN . /opt/conda/etc/profile.d/conda.sh && \
    conda install -n base conda-forge::starship -y

USER jupyter
WORKDIR /home/jupyter

COPY environment.yml .

RUN . /opt/conda/etc/profile.d/conda.sh && \
    conda init zsh && \
    conda env create --file environment.yml

RUN echo "conda activate --stack jupyter" >> /home/jupyter/.zshrc && \
    echo "eval \"\$(starship init zsh)\"" >> /home/jupyter/.zshrc
