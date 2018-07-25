FROM multiarch/debian-debootstrap:armhf-jessie

RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        curl \
        libatlas3-base \
        libfreetype6-dev \
        libpng12-dev \
        libzmq3-dev \
        pkg-config \
        python3 \
        python3-dev \
        rsync \
        unzip \
    && apt-get clean && \
        rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1

WORKDIR /tmp
COPY get-pip.py /tmp/
RUN python /tmp/get-pip.py && \
        rm /tmp/get-pip.py

COPY pip.conf /etc/pip.conf
RUN pip --no-cache-dir install "numpy==1.14.5"
RUN pip --no-cache-dir install \
        ipykernel \
        jupyter \
        matplotlib && \
            python -m ipykernel.kernelspec
RUN pip --no-cache-dir install "tensorflow==1.9.0"

COPY jupyter_notebook_config.py /root/.jupyter/

# Copy sample notebooks.
COPY notebooks /notebooks

# Jupyter has issues with being run directly:
#   https://github.com/ipython/ipython/issues/7062
# We just add a little wrapper script.
COPY run_jupyter.sh /

# TensorBoard
EXPOSE 6006
# IPython
EXPOSE 8888

WORKDIR "/notebooks"

CMD ["/run_jupyter.sh", "--allow-root"]
