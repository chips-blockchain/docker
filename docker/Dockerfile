FROM ubuntu:18.04
ENV BUILD_PACKAGES="software-properties-common autoconf git build-essential libtool libprotobuf-c-dev libgmp-dev libsqlite3-dev python python3 zip libevent-dev pkg-config libssl-dev libcurl4-gnutls-dev make libboost-all-dev automake jq wget ninja-build libsqlite3-dev libgmp3-dev valgrind libcli-dev libsecp256k1-dev libsodium-dev libbase58-dev nano tmux python3-mako curl"

#BET repos
ENV LIGHTNING_REPO https://github.com/chips-blockchain/lightning
ENV CHIPS_REPO https://github.com/chips-blockchain/chips
ENV BET https://github.com/chips-blockchain/bet

#Dependencies 
ENV NANOMSG https://github.com/nanomsg/nng.git
ENV LIBWEBSOCKETS https://github.com/sg777/libwebsockets.git
ENV LIBWALLY https://github.com/ElementsProject/libwally-core.git

RUN apt-get update && \
  apt-get install -y $BUILD_PACKAGES

# nanomsg-next-generation requires cmake 3.13 or higher
ADD https://cmake.org/files/v3.16/cmake-3.16.1-Linux-x86_64.sh /cmake-3.16.1-Linux-x86_64.sh
RUN mkdir /opt/cmake && \
  sh /cmake-3.16.1-Linux-x86_64.sh --prefix=/opt/cmake --skip-license && \
  ln -s /opt/cmake/bin/cmake /usr/local/bin/cmake

######### CHIPS #########

# Source: https://cryptoandcoffee.com/mining-gems/install-berkeley-4-8-db-libs-on-ubuntu-16-04/
RUN mkdir ~/db-4.8.30
ADD db-4.8.30.NC.tar.gz ~/db-4.8.30

RUN  cd /~/db-4.8.30/db-4.8.30.NC/build_unix && \ 
  ../dist/configure -enable-cxx -disable-shared -with-pic -prefix=/chips/db4 && \
  make -j2 && \
  make install 

RUN cd ~ && \
  git clone $CHIPS_REPO && \
  cd chips && \
  git checkout master && \
  ./build.sh

# Bootstraping CHIPS

RUN  mkdir ~/.chips && cd ~/.chips &&  \
  wget http://bootstrap3rd.dexstats.info/CHIPS-bootstrap.tar.gz && \
  tar xvzf CHIPS-bootstrap.tar.gz && \
  rm CHIPS-bootstrap.tar.gz

RUN mkdir -p $HOME/.chips/ 
RUN echo $"server=1 \n\
        daemon=1 \n\
        txindex=1 \n\
        rpcuser=$(openssl rand -base64 12) \n\
        rpcpassword=$(openssl rand -base64 12) \n\
        addnode=159.69.23.29 \n\
        addnode=95.179.192.102 \n\
        addnode=149.56.29.163 \n\
        addnode=145.239.149.173 \n\
        addnode=178.63.53.110 \n\
        addnode=151.80.108.76 \n\
        addnode=185.137.233.199 \n\
        rpcbind=127.0.0.1 \n\
        rpcallowip=127.0.0.1" > $HOME/.chips/chips.conf

######### LIGHTNING #########

RUN cd ~ && git clone $LIGHTNING_REPO
RUN cd ~/lightning && \
  make

RUN ln -sf ~/chips/src/chipsd /usr/local/bin/chipsd && \
  ln -sf ~/chips/src/chips-cli /usr/local/bin/chips-cli && \
  ln -sf ~/lightning/lightningd/lightningd /usr/local/bin/lightning && \
  ldconfig /usr/local/lib

# lightning-cli terminal shortcut
RUN cd /usr/bin/ && \
  echo "#/bin/bash \n ~/lightning/cli/lightning-cli \$1 \$2 \$3 \$4 \$5 \$6 | jq ." > lightning-cli && \
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
      git clone $LIBWALLY && \
      cd libwally-core && \
      git submodule update --init --recursive && \
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

CMD ["/bin/bash"]