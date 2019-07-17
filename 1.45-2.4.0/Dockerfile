### LMNtal Dockerfile
### lmntal-compiler: 1.45
### slim: 2.4.0

# LMNtal compiler build

FROM frekele/ant:1.10.3-jdk8 AS lmntal-builder
RUN cd / && \
    wget https://github.com/lmntal/lmntal-compiler/archive/v1.45.tar.gz -O lmntal-compiler.tar.gz && \
    tar xvzf lmntal-compiler.tar.gz && \
    cd lmntal-compiler-1.45 && \
    ant && \
    ant archive && \
    mkdir /lmntal-build && \
    cd /lmntal-build && \
    tar xvzf /lmntal-compiler-1.45/lmntal.tgz


# slim build

FROM openjdk:8-jre-slim-stretch AS slim-builder
ENV LMNTAL_HOME=/usr/local
RUN apt update && \
    apt install -y automake bison build-essential ca-certificates flex libtool re2c ruby wget --no-install-recommends && \
    wget https://github.com/lmntal/slim/archive/v2.4.0.tar.gz -O slim.tar.gz && \
    rm -rf /var/lib/apt/lists/* && \
    tar xzf slim.tar.gz && \
    cd slim-2.4.0 && \
    ./autogen.sh && \
    ./configure --prefix=/slim-build
COPY --from=lmntal-builder /lmntal-build/* /usr/local/
RUN cd /slim-2.4.0 && \
    make && \
    make install


# Runtime Container

FROM openjdk:8-jre-slim-stretch

ENV LANG=C.UTF-8
ENV LMNTAL_HOME=/usr/local

RUN apt update && \
    apt install -y libgomp1 --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

COPY --from=lmntal-builder /lmntal-build/* /usr/local/
COPY --from=slim-builder /slim-build/* /usr/local/

CMD ["/bin/sh", "-c", "lmntal --stdin-lmn --slimcode --hl-opt | slim --hl --hide-ruleset -"]
