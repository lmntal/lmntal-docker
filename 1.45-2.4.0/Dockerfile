FROM openjdk:8-jre-slim

ENV LANG=C.UTF-8
ENV LMNTAL_HOME=/usr/local

RUN apt update && \
    apt install -y libgomp1 --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

ADD lmntal-1.45.tgz /usr/local/
ADD slim-2.4.0.tgz /usr/local/

CMD ["/bin/sh", "-c", "lmntal --stdin-lmn | slim -"]
