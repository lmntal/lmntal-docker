### LMNtal Dockerfile
### lmntal-compiler: 1.44
### slim: 2.3.1

# LMNtal compiler build

FROM frekele/ant:1.10.3-jdk8 AS lmntal-builder
RUN cd / && \
    wget https://github.com/lmntal/lmntal-compiler/archive/v1.44.tar.gz -O lmntal-compiler.tar.gz && \
    tar xvzf lmntal-compiler.tar.gz && \
    cd lmntal-compiler-1.44 && \
    ant && \
    ant archive && \
    mkdir /lmntal-build && \
    cd /lmntal-build && \
    tar xvzf /lmntal-compiler-1.44/lmntal.tgz


# slim build

FROM openjdk:8-jre-slim-stretch AS slim-builder
ENV LMNTAL_HOME=/usr/local
RUN apt update && \
    apt install -y automake bison build-essential ca-certificates flex libtool re2c ruby wget --no-install-recommends && \
    wget https://github.com/lmntal/slim/archive/v2.3.1.tar.gz -O slim.tar.gz && \
    rm -rf /var/lib/apt/lists/* && \
    tar xzf slim.tar.gz && \
    cd slim-2.3.1 && \
    aclocal && \
    autoheader && \
    automake --add-missing && \
    autoreconf && \
    ./configure --prefix=/slim-build
COPY --from=lmntal-builder /lmntal-build/* /usr/local/
RUN cd /slim-2.3.1 && \
    make && \
    make install


# Runtime Container

FROM openjdk:8-jre-slim-stretch

ENV LANG=C.UTF-8
ENV LMNTAL_HOME=/usr/local
ENV LD_LIBRARY_PATH=/usr/local/lib

RUN apt update && \
    apt install -y libgomp1 --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

COPY --from=lmntal-builder /lmntal-build/* /usr/local/
COPY --from=slim-builder /slim-build/* /usr/local/

CMD ["/bin/sh", "-c", "lmntal --stdin-lmn | slim -"]
