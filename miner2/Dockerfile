FROM ubuntu:focal
ENV TZ='Europe/Berlin'

RUN apt-get update \
     && apt-get install -y wget software-properties-common

WORKDIR "/root"
RUN add-apt-repository -y ppa:ethereum/ethereum

RUN apt-get update \
     && apt-get install -y ethereum

ARG password
ARG privatekey
RUN echo $password > ~/.accountpassword
RUN echo $privatekey > ~/.privatekey
ADD ./genesis.json ./genesis.json
RUN geth init genesis.json
RUN geth account import --password ~/.accountpassword  ~/.privatekey

ENV address=""
ENV bootnodeId=""
ENV bootnodeIp=""

CMD exec geth --bootnodes "enode://$bootnodeId@$bootnodeIp:30301" --networkid "1337" --verbosity=4 --syncmode=full --mine --miner.gasprice "0" --miner.etherbase $address --unlock $address --password ~/.accountpassword --http --http.addr "0.0.0.0" --http.port 8551 --http.api "eth,web3,net,admin,debug,miner,personal" --http.corsdomain "*" --ws --ws.addr "0.0.0.0" --ws.api "eth,web3,net,admin,debug,miner,personal" --ws.port 8552 --ws.origins "*" --allow-insecure-unlock

EXPOSE 8551
EXPOSE 8552
EXPOSE 30303