# FROM ubuntu:18.04

# RUN apt-get update && \
#   apt-get install -y curl git apt-utils libpython-stdlib libpython2.7-minimal libpython2.7-stdlib nodejs python python-minimal python2.7 python2.7-minimal

# RUN cd ~ && curl -sL https://deb.nodesource.com/setup_12.x | bash
# RUN cd ~ && apt-get install nodejs
# RUN cd ~ && git clone https://github.com/chips-blockchain/pangea-poker.git && cd ~/pangea-poker && npm install && npm run dev

# EXPOSE 1234

FROM node:12
RUN cd ~ && git clone https://github.com/chips-blockchain/pangea-poker.git && cd ~/pangea-poker && npm install

EXPOSE 1234
WORKDIR "/root/pangea-poker"
CMD [ "npm", "run", "prod" ]