FROM ubuntu:jammy

ARG BRANCH=master

USER root

WORKDIR /opt
RUN apt-get update && \
    apt-get install -y --no-install-recommends openssl ca-certificates iputils-ping pkg-config git libicu-dev python3-dev python3-pip polyglot python3-icu make gcc g++ supervisor && \
    update-alternatives --install /usr/bin/python python /usr/bin/python3 10 && \
    git clone https://github.com/aboSamoor/pycld2.git && \
    git clone https://github.com/mikeweb85/nlpserver.git

WORKDIR /opt/pycld2
RUN make clean && \
    make build

WORKDIR /opt/nlpserver
RUN git fetch origin && \
    git checkout ${BRANCH} && \
    pip install -r requirements.txt && \
    python -m polyglot download LANG:en && \
    pip3 install -U spacy && \
    python -m spacy download en_core_web_sm && \
    python -m spacy download xx_ent_wiki_sm

COPY nlpserver.conf /etc/supervisor/conf.d
COPY entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/entrypoint.sh

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

EXPOSE 6400