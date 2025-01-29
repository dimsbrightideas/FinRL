

FROM stablebaselines/rl-baselines3-zoo
#set envionment variables
USER root

RUN set -xe \
    && apt-get update \
    && apt-get install -y python3-pip

WORKDIR /src

# RUN pip install swig
# RUN pip install box2d
# RUN pip install git+https://github.com/AI4Finance-Foundation/FinRL.git

COPY requirements.txt .

RUN pip install -r requirements.txt -I --break-system-packages


RUN pip install jupyterlab --break-system-packages

# Add Tini. Tini operates as a process subreaper for jupyter. This prevents kernel crashes.
ENV TINI_VERSION v0.6.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod +x /usr/bin/tini

# copy the code instead of mounting
COPY . .

ENTRYPOINT ["/usr/bin/tini", "--"]

CMD ["python", "jupyter", "lab", "--port=8888", "--no-browser", "--ip=0.0.0.0", "--NotebookApp.token=''","--NotebookApp.password=''","--allow-root"]

