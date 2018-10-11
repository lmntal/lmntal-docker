FROM buildpack-deps:stretch-curl

ENV LMNTAL_HOME /usr/lmntal
ENV PATH /usr/local/bin:/usr/lmntal/bin:$PATH

ENV LANG C.UTF-8

RUN set -ex; \
        apt-get update \
        && apt-get install -y --no-install-recommends \
            ant \
            autoconf \
            automake \
            bison \
            flex \
            g++ \
            gcc \
            openjdk-8-jdk-headless \
            libtool \
            make \
            re2c \
            ruby \
        \
        && wget -O lmntal.tar.gz "https://github.com/lmntal/lmntal-compiler/archive/v1.44.tar.gz" \
        && wget -O slim.tar.gz "https://github.com/lmntal/slim/archive/v2.3.1.tar.gz" \
        && tar -xzf lmntal.tar.gz \
        && mv lmntal-compiler-1.44 "$LMNTAL_HOME" \
        && rm lmntal.tar.gz \
        && export SLIM_HOME="/tmp/slim" \
        && tar -xzf slim.tar.gz \
        && mv slim-2.3.1 "$SLIM_HOME" \
        && rm slim.tar.gz \
        \
        && cd "$LMNTAL_HOME" \
        && ant \
        \
        && cd "$SLIM_HOME" \
        && aclocal \
        && autoheader \
        && automake --add-missing \
        && autoreconf \
        && ./configure --prefix=/usr/local \
        && make install \
        \
        && ldconfig; \
        \
        rm -rf /var/lib/apt/lists/*


CMD ["/bin/sh", "-c", "lmntal --stdin-lmn | slim -"]
