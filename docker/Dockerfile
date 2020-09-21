FROM ubuntu:18.04
ENV BUILD_PACKAGES="software-properties-common autoconf git build-essential libtool libprotobuf-c-dev libgmp-dev libsqlite3-dev python python3 zip libevent-dev pkg-config libssl-dev libcurl4-gnutls-dev make libboost-all-dev automake jq wget ninja-build libsqlite3-dev libgmp3-dev valgrind libcli-dev libsecp256k1-dev libsodium-dev libbase58-dev nano tmux"

#BET repos
ENV LIGHTNING_REPO https://github.com/chips-blockchain/lightning
ENV CHIPS_REPO https://github.com/chips-blockchain/chips
ENV BET https://github.com/chips-blockchain/bet

#Dependencies 
ENV NANOMSG https://github.com/nanomsg/nng.git
ENV LIBWEBSOCKETS https://github.com/sg777/libwebsockets.git
ENV JSMN https://github.com/zserge/jsmn.git
ENV LIBWALLY https://github.com/ElementsProject/libwally-core.git

RUN apt-get update && \
  apt-get install -y $BUILD_PACKAGES

# nanomsg-next-generation requires cmake 3.13 or higher
ADD https://cmake.org/files/v3.16/cmake-3.16.1-Linux-x86_64.sh /cmake-3.16.1-Linux-x86_64.sh
RUN mkdir /opt/cmake && \
  sh /cmake-3.16.1-Linux-x86_64.sh --prefix=/opt/cmake --skip-license && \
  ln -s /opt/cmake/bin/cmake /usr/local/bin/cmake

######### CHIPS #########

RUN cd ~ && \
  git clone $CHIPS_REPO && \
  cd chips && \
  git checkout master

# Source: https://cryptoandcoffee.com/mining-gems/install-berkeley-4-8-db-libs-on-ubuntu-16-04/
RUN mkdir ~/db-4.8.30
ADD db-4.8.30.NC.tar.gz ~/db-4.8.30

RUN  cd /~/db-4.8.30/db-4.8.30.NC/build_unix && \ 
  ../dist/configure -enable-cxx -disable-shared -with-pic -prefix=/chips/db4 && \
  make -j2 && \
  make install 

RUN cd ~/chips && \
  ./autogen.sh && \
  ./configure LDFLAGS="-L/chips/db4/lib/" CPPFLAGS="-I/chips/db4/include/" -without-gui -without-miniupnpc --disable-tests --disable-bench --with-gui=no && \
  make -j2

# Bootstraping CHIPS

RUN  mkdir ~/.chips && cd ~/.chips && \
  wget http://bootstrap3rd.dexstats.info/CHIPS-bootstrap.tar.gz && \
  tar xvzf CHIPS-bootstrap.tar.gz && \
  rm CHIPS-bootstrap.tar.gz

######### LIGHTNING #########

RUN cd ~ && git clone $LIGHTNING_REPO
RUN cd ~/lightning && \
  make

RUN ln -sf ~/chips/src/chipsd /usr/local/bin/chipsd && \
  ln -sf ~/chips/src/chips-cli /usr/local/bin/chips-cli && \
  ln -sf ~/lightning/lightningd/lightningd /usr/local/bin/lightning

# lightning-cli terminal shortcut
RUN cd /usr/bin/ && \
  echo "#/bin/bash \n ~/lightning/cli/lightning-cli $1 $2 $3 $4 $5 $6 | jq ." > lightning-cli && \
  chmod +x /usr/bin/lightning-cli

########### BET ############

RUN cd ~ && \
 git clone $NANOMSG && \
 cd nng && \
 mkdir build && \
 cd build && \
 cmake -G Ninja .. && \
 cd ~/nng/build && \
 ls -l && \
 ninja install

# Installing libwebsockets

RUN cd ~ && \
  git clone $LIBWEBSOCKETS && \
  cd libwebsockets && \
  mkdir build && \
  cd build && \
  cmake -DLWS_WITH_HTTP2=1 .. && \
  make && \
  make install && \
  ldconfig /usr/local/lib

#Installing Bet

# bet dependency
RUN cd ~ && \
    git clone $JSMN && \
    cd jsmn && make

# bet dependency
RUN cd ~ && \
      git clone $LIBWALLY && \
      cd libwally-core && \
      ./tools/autogen.sh && \
      ./configure && \
      make && \
      make check

RUN cd ~ && \
  git clone $BET && \
  cd bet && \
  make

# EXPOSE rpc port for the node to allow outside container access
EXPOSE 57777 9735