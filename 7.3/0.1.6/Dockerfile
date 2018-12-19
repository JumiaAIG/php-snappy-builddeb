FROM marceloalmeida/php-docker:7.2

MAINTAINER Marcelo Almeida <marcelo.almeida@jumia.com>

WORKDIR "/root"

ENV DEBIAN_FRONTEND noninteractive
ENV VERSION 0.1.6

# INSTALL BUILDER DEPENDENCIES
RUN apt-get update && apt-get install -y --no-install-recommends \
  apt-utils \
  build-essential \
  ca-certificates \
  checkinstall \
  git \
  lsb-release \
  make \
  php7.3-dev \
  re2c \
  runawk

COPY src /src

# INSTALL PACKAGES DEPENDENCIES
RUN mkdir /pkg
RUN apt-get install -y libsnappy-dev

# CREATE PACKAGE
RUN \
  git clone https://github.com/kjdev/php-ext-snappy.git && \
  cd php-ext-snappy && \
  git checkout tags/$VERSION && \
  git submodule update --init && \
  cp -r /src/* /root/php-ext-snappy/. && \
  phpize && \
  ./configure && \
  checkinstall --install=no --pkgname='php7.3-snappy' --pkgversion='$VERSION' --pkggroup='php' --pkgsource='https://github.com/kjdev/php-ext-snappy' --maintainer='Jumia TechOps \<techops@jumia.com\>' --requires='php7.3-common \(\>= 7.3.0\), libsnappy1 \(\>= 1.1.2\)' --include=include_etc

# PERFORM TEST
RUN cd php-ext-snappy ;\
  make && make test

VOLUME ["/pkg"]
