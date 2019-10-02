# cowsay-hyperledger

Hyperledger Foundation: hands on

```bash
FIAP - Hyperledger Foundation

< Aula: Hyperledger Foundation -
< Prof.: Arthur Miranda de Souza >
 -----------------------------
     \   ^__^
      \  (oo)\_______
         (__)\       )\/\
            ||----w |
            ||     ||
by Renato Puga

```

## Ajuda

* http://dontpad.com/fiap1blc

## Docker Commands by Arthur

```bash
 ________________________________
<  Matando as coisas docker tudo  >
 --------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

```

```bash
# mantando os container e imagens
docker rm -f $(docker ps -a -q)

# matando as imagens
sudo docker rmi -f $(docker images -a -q)
```

## Aula 02

```bash
# volta pra casa
cd
cd fabric-samples/first-network/

# gera o diretorio crypto-config	
. byfn.sh generate

# listando conteudo do crypto-config
ls crypto-config
ordererOrganizations  peerOrganizations

# subindo os containers
. byfn.sh up
```
```bash
 ____________________
< Subimos dois Peers >
 --------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

```
Vamos entrar no CLI:

```bash
# verificando os processos do docker
docker ps

CONTAINER ID        IMAGE                             COMMAND             CREATED             STATUS              PORTS                                              NAMES
c3bd1974e487        hyperledger/fabric-tools:latest   "/bin/bash"         42 seconds ago      Up 41 seconds                                                          cli
bdaf689e82d3        hyperledger/fabric-peer:latest    "peer node start"   46 seconds ago      Up 43 seconds       0.0.0.0:10051->7051/tcp, 0.0.0.0:10053->7053/tcp   peer1.org2.example.com
2704cfc44fb6        hyperledger/fabric-peer:latest    "peer node start"   46 seconds ago      Up 43 seconds       0.0.0.0:8051->7051/tcp, 0.0.0.0:8053->7053/tcp     peer1.org1.example.com
b4d93a728056        hyperledger/fabric-peer:latest    "peer node start"   46 seconds ago      Up 42 seconds       0.0.0.0:7051->7051/tcp, 0.0.0.0:7053->7053/tcp     peer0.org1.example.com
e71060f2ae89        hyperledger/fabric-peer:latest    "peer node start"   46 seconds ago      Up 42 seconds       0.0.0.0:9051->7051/tcp, 0.0.0.0:9053->7053/tcp     peer0.org2.example.com

# acesando o cli
docker exec -it cli bash

# verificando status do peer 	
# root@c3bd1974e487:/opt/gopath/src/github.com/hyperledger/fabric/peer# 
peer node status

2019-09-26 23:30:45.375 UTC [main] InitCmd -> WARN 001 CORE_LOGGING_LEVEL is no longer supported, please use the FABRIC_LOGGING_SPEC environment variable
status:STARTED 

# verificando se existe algum chaincode instanciado
# root@c3bd1974e487:/opt/gopath/src/github.com/hyperledger/fabric/peer# 
peer chaincode list --installed

2019-09-26 23:32:59.641 UTC [main] InitCmd -> WARN 001 CORE_LOGGING_LEVEL is no longer supported, please use the FABRIC_LOGGING_SPEC environment variable
2019-09-26 23:32:59.644 UTC [main] SetOrdererEnv -> WARN 002 CORE_LOGGING_LEVEL is no longer supported, please use the FABRIC_LOGGING_SPEC environment variable
Get installed chaincodes on peer:
```

```bash
 ____________________
< Tudo bem se o chaincode >
< nao estiver instanciado >
 --------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

```

**NOTA:** Estavamos animados para executar mais um novo comando, porém, deu essa mensagem: 

```bash
#root@c3bd1974e487:/opt/gopath/src/github.com/hyperledger/fabric/peer# 
peer chaincode query -C mychannel -n mycc -c '{"Args":["query","a"]}'

2019-09-26 23:42:41.755 UTC [main] InitCmd -> WARN 001 CORE_LOGGING_LEVEL is no longer supported, please use the FABRIC_LOGGING_SPEC environment variable
2019-09-26 23:42:41.758 UTC [main] SetOrdererEnv -> WARN 002 CORE_LOGGING_LEVEL is no longer supported, please use the FABRIC_LOGGING_SPEC environment variable
Error: error endorsing query: rpc error: code = Unknown desc = access denied: channel [mychannel] creator org [Org1MSP] - proposal response: <nil>

```

```bash
 __________________________
< Bora usar um comando      >
< docker logs para debugar? >
 --------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

```
Em um nova aba do terminal execute:

```bash
# logs no orderer 
# se der tudo certo, aparece a mensagem abaixo
docker logs orderer.example.com 

2019-09-26 22:58:27.957 UTC [localconfig] completeInitialization -> INFO 001 Kafka.Version unset, setting to 0.10.2.0
2019-09-26 22:58:28.010 UTC [orderer.common.server] prettyPrintStruct -> INFO 002 Orderer config values:
	General.LedgerType = "file"
	General.ListenAddress = "0.0.0.0"
	General.ListenPort = 7050
	General.TLS.Enabled = true
	General.TLS.PrivateKey = "/var/hyperledger/orderer/tls/server.key"
	General.TLS.Certificate = "/var/hyperledger/orderer/tls/server.crt"
	General.TLS.RootCAs = [/var/hyperledger/orderer/tls/ca.crt]
	General.TLS.ClientAuthRequired = false
	General.TLS.ClientRootCAs = []
	General.Cluster.ListenAddress = ""
	General.Cluster.ListenPort = 0
	General.Cluster.ServerCertificate = ""
	General.Cluster.ServerPrivateKey = ""
	General.Cluster.ClientCertificate = ""
	General.Cluster.ClientPrivateKey = ""
	General.Cluster.RootCAs = []
	General.Cluster.DialTimeout = 5s
	General.Cluster.RPCTimeout = 7s
	General.Cluster.ReplicationBufferSize = 20971520
	General.Cluster.ReplicationPullTimeout = 5s
	General.Cluster.ReplicationRetryTimeout = 5s
	General.Cluster.ReplicationBackgroundRefreshInterval = 5m0s
	General.Cluster.ReplicationMaxRetries = 12
	General.Cluster.SendBufferSize = 10
	General.Cluster.CertExpirationWarningThreshold = 168h0m0s
	General.Cluster.TLSHandshakeTimeShift = 0s
	General.Keepalive.ServerMinInterval = 1m0s
	General.Keepalive.ServerInterval = 2h0m0s
	General.Keepalive.ServerTimeout = 20s
	General.ConnectionTimeout = 0s
	General.GenesisMethod = "file"
	General.GenesisProfile = "SampleInsecureSolo"
	General.SystemChannel = "test-system-channel-name"
	General.GenesisFile = "/var/hyperledger/orderer/orderer.genesis.block"
	General.Profile.Enabled = false
	General.Profile.Address = "0.0.0.0:6060"
	General.LocalMSPDir = "/var/hyperledger/orderer/msp"
	General.LocalMSPID = "OrdererMSP"
	General.BCCSP.ProviderName = "SW"
	General.BCCSP.SwOpts.SecLevel = 256
	General.BCCSP.SwOpts.HashFamily = "SHA2"
	General.BCCSP.SwOpts.Ephemeral = false
	General.BCCSP.SwOpts.FileKeystore.KeyStorePath = "/var/hyperledger/orderer/msp/keystore"
	General.BCCSP.SwOpts.DummyKeystore =
	General.BCCSP.SwOpts.InmemKeystore =
	General.BCCSP.PluginOpts =
	General.Authentication.TimeWindow = 15m0s
	General.Authentication.NoExpirationChecks = false
	FileLedger.Location = "/var/hyperledger/production/orderer"
	FileLedger.Prefix = "hyperledger-fabric-ordererledger"
	RAMLedger.HistorySize = 1000
	Kafka.Retry.ShortInterval = 5s
	Kafka.Retry.ShortTotal = 10m0s
	Kafka.Retry.LongInterval = 5m0s
	Kafka.Retry.LongTotal = 12h0m0s
	Kafka.Retry.NetworkTimeouts.DialTimeout = 10s
	Kafka.Retry.NetworkTimeouts.ReadTimeout = 10s
	Kafka.Retry.NetworkTimeouts.WriteTimeout = 10s
	Kafka.Retry.Metadata.RetryMax = 3
	Kafka.Retry.Metadata.RetryBackoff = 250ms
	Kafka.Retry.Producer.RetryMax = 3
	Kafka.Retry.Producer.RetryBackoff = 100ms
	Kafka.Retry.Consumer.RetryBackoff = 2s
	Kafka.Verbose = false
	Kafka.Version = 0.10.2.0
	Kafka.TLS.Enabled = false
	Kafka.TLS.PrivateKey = ""
	Kafka.TLS.Certificate = ""
	Kafka.TLS.RootCAs = []
	Kafka.TLS.ClientAuthRequired = false
	Kafka.TLS.ClientRootCAs = []
	Kafka.SASLPlain.Enabled = false
	Kafka.SASLPlain.User = ""
	Kafka.SASLPlain.Password = ""
	Kafka.Topic.ReplicationFactor = 3
	Debug.BroadcastTraceDir = ""
	Debug.DeliverTraceDir = ""
	Consensus = map[WALDir:/var/hyperledger/production/orderer/etcdraft/wal SnapDir:/var/hyperledger/production/orderer/etcdraft/snapshot]
	Operations.ListenAddress = "127.0.0.1:8443"
	Operations.TLS.Enabled = false
	Operations.TLS.PrivateKey = ""
	Operations.TLS.Certificate = ""
	Operations.TLS.RootCAs = []
	Operations.TLS.ClientAuthRequired = false
	Operations.TLS.ClientRootCAs = []
	Metrics.Provider = "disabled"
	Metrics.Statsd.Network = "udp"
	Metrics.Statsd.Address = "127.0.0.1:8125"
	Metrics.Statsd.WriteInterval = 30s
	Metrics.Statsd.Prefix = ""
panic: unable to bootstrap orderer. Error reading genesis block file: read /var/hyperledger/orderer/orderer.genesis.block: is a directory

goroutine 1 [running]:
github.com/hyperledger/fabric/orderer/common/bootstrap/file.(*fileBootstrapper).GenesisBlock(0xc000163f60, 0xc000163f60)
	/opt/gopath/src/github.com/hyperledger/fabric/orderer/common/bootstrap/file/bootstrap.go:39 +0x1d0
github.com/hyperledger/fabric/orderer/common/server.extractBootstrapBlock(0xc000288900, 0x0)
	/opt/gopath/src/github.com/hyperledger/fabric/orderer/common/server/main.go:532 +0x1bd
github.com/hyperledger/fabric/orderer/common/server.Start(0x1018e03, 0x5, 0xc000288900)
	/opt/gopath/src/github.com/hyperledger/fabric/orderer/common/server/main.go:96 +0x43
github.com/hyperledger/fabric/orderer/common/server.Main()
	/opt/gopath/src/github.com/hyperledger/fabric/orderer/common/server/main.go:91 +0x1ce
main.main()
	/opt/gopath/src/github.com/hyperledger/fabric/orderer/main.go:15 +0x20

```

Na aba com o docker bash...

```bash
# exportar o channel
export CHANNEL_NAME=mychannel

# testando a variavel de ambiente
echo $CHANNEL_NAME
mychannel

# tentando rodar invoke
# pegamos esse comando aqui:
# https://hyperledger-fabric.readthedocs.io/en/release-1.4/build_network.html
peer chaincode invoke -o orderer.example.com:7050 --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C $CHANNEL_NAME -n mycc --peerAddresses peer0.org1.example.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses peer0.org2.example.com:9051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"Args":["invoke","a","b","10"]}'

2019-09-27 00:07:53.324 UTC [main] InitCmd -> WARN 001 CORE_LOGGING_LEVEL is no longer supported, please use the FABRIC_LOGGING_SPEC environment variable
2019-09-27 00:07:53.327 UTC [main] SetOrdererEnv -> WARN 002 CORE_LOGGING_LEVEL is no longer supported, please use the FABRIC_LOGGING_SPEC environment variable
Error: error getting endorser client for invoke: endorser client failed to connect to peer0.org2.example.com:9051: failed to create new connection: connection error: desc = "transport: error while dialing: dial tcp 172.19.0.5:9051: connect: connection refused"
```


```bash
 ____________________________
< connection refused #xatiado >
 ----------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

```

## Aula 03

Como eu cheguei atrasado eu tive que correr para chegar na mesma pagina que todos. Entao, tive que gerar artefatos de rede, basicamente partindo do zero. Na VM, executei os seguintes comandos:

```bash
# voltei para o home
cd 

# acessando o fabric-samples diretorio first-network
cd fabric-samples/first-network/
```

Agora, estou pronto para gerar manualmente os artefados:

```bash
sudo ./byfn.sh generate
Generating certs and genesis block for with channel 'mychannel' and CLI timeout of '10' seconds and CLI delay of '3' seconds
Continue? [Y/n] Y
proceeding ...
/home/aluno/fabric-samples/first-network/../bin/cryptogen

##########################################################
##### Generate certificates using cryptogen tool #########
##########################################################
+ cryptogen generate --config=./crypto-config.yaml
org1.example.com
org2.example.com
+ res=0
+ set +x

/home/aluno/fabric-samples/first-network/../bin/configtxgen
##########################################################
#########  Generating Orderer Genesis block ##############
##########################################################
+ configtxgen -profile TwoOrgsOrdererGenesis -outputBlock ./channel-artifacts/genesis.block
2019-10-01 22:50:56.128 -03 [common/tools/configtxgen] main -> INFO 001 Loading configuration
2019-10-01 22:50:56.141 -03 [msp] getMspConfig -> INFO 002 Loading NodeOUs
2019-10-01 22:50:56.141 -03 [msp] getMspConfig -> INFO 003 Loading NodeOUs
2019-10-01 22:50:56.141 -03 [common/tools/configtxgen] doOutputBlock -> INFO 004 Generating genesis block
2019-10-01 22:50:56.142 -03 [common/tools/configtxgen] doOutputBlock -> INFO 005 Writing genesis block
+ res=0
+ set +x

#################################################################
### Generating channel configuration transaction 'channel.tx' ###
#################################################################
+ configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID mychannel
2019-10-01 22:50:56.149 -03 [common/tools/configtxgen] main -> INFO 001 Loading configuration
2019-10-01 22:50:56.153 -03 [common/tools/configtxgen] doOutputChannelCreateTx -> INFO 002 Generating new channel configtx
2019-10-01 22:50:56.153 -03 [msp] getMspConfig -> INFO 003 Loading NodeOUs
2019-10-01 22:50:56.154 -03 [msp] getMspConfig -> INFO 004 Loading NodeOUs
2019-10-01 22:50:56.179 -03 [common/tools/configtxgen] doOutputChannelCreateTx -> INFO 005 Writing new channel tx
+ res=0
+ set +x

#################################################################
#######    Generating anchor peer update for Org1MSP   ##########
#################################################################
+ configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org1MSPanchors.tx -channelID mychannel -asOrg Org1MSP
2019-10-01 22:50:56.185 -03 [common/tools/configtxgen] main -> INFO 001 Loading configuration
2019-10-01 22:50:56.189 -03 [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 002 Generating anchor peer update
2019-10-01 22:50:56.198 -03 [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 003 Writing anchor peer update
+ res=0
+ set +x

#################################################################
#######    Generating anchor peer update for Org2MSP   ##########
#################################################################
+ configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org2MSPanchors.tx -channelID mychannel -asOrg Org2MSP
2019-10-01 22:50:56.207 -03 [common/tools/configtxgen] main -> INFO 001 Loading configuration
2019-10-01 22:50:56.211 -03 [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 002 Generating anchor peer update
2019-10-01 22:50:56.211 -03 [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 003 Writing anchor peer update
+ res=0
+ set +x

```

```bash
 _________________________________
< A bebida me disse pra usar sudo >
 ---------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

```

Agora, vamos trazer a rede de volta novamente:

```bash
sudo ./byfn.sh up
Starting with channel 'mychannel' and CLI timeout of '10' seconds and CLI delay of '3' seconds
Continue? [Y/n] Y
proceeding ...
2019-10-02 01:53:57.270 UTC [main] main -> INFO 001 Exiting.....
LOCAL_VERSION=1.1.0
DOCKER_IMAGE_VERSION=1.1.0
/home/aluno/fabric-samples/first-network/../bin/cryptogen

##########################################################
##### Generate certificates using cryptogen tool #########
##########################################################
+ cryptogen generate --config=./crypto-config.yaml
org1.example.com
org2.example.com
+ res=0
+ set +x

/home/aluno/fabric-samples/first-network/../bin/configtxgen
##########################################################
#########  Generating Orderer Genesis block ##############
##########################################################
+ configtxgen -profile TwoOrgsOrdererGenesis -outputBlock ./channel-artifacts/genesis.block
2019-10-01 22:53:57.640 -03 [common/tools/configtxgen] main -> INFO 001 Loading configuration
2019-10-01 22:53:57.645 -03 [msp] getMspConfig -> INFO 002 Loading NodeOUs
2019-10-01 22:53:57.645 -03 [msp] getMspConfig -> INFO 003 Loading NodeOUs
2019-10-01 22:53:57.645 -03 [common/tools/configtxgen] doOutputBlock -> INFO 004 Generating genesis block
2019-10-01 22:53:57.645 -03 [common/tools/configtxgen] doOutputBlock -> INFO 005 Writing genesis block
+ res=0
+ set +x

#################################################################
### Generating channel configuration transaction 'channel.tx' ###
#################################################################
+ configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID mychannel
2019-10-01 22:53:57.654 -03 [common/tools/configtxgen] main -> INFO 001 Loading configuration
2019-10-01 22:53:57.668 -03 [common/tools/configtxgen] doOutputChannelCreateTx -> INFO 002 Generating new channel configtx
2019-10-01 22:53:57.669 -03 [msp] getMspConfig -> INFO 003 Loading NodeOUs
2019-10-01 22:53:57.669 -03 [msp] getMspConfig -> INFO 004 Loading NodeOUs
2019-10-01 22:53:57.690 -03 [common/tools/configtxgen] doOutputChannelCreateTx -> INFO 005 Writing new channel tx
+ res=0
+ set +x

#################################################################
#######    Generating anchor peer update for Org1MSP   ##########
#################################################################
+ configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org1MSPanchors.tx -channelID mychannel -asOrg Org1MSP
2019-10-01 22:53:57.699 -03 [common/tools/configtxgen] main -> INFO 001 Loading configuration
2019-10-01 22:53:57.704 -03 [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 002 Generating anchor peer update
2019-10-01 22:53:57.704 -03 [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 003 Writing anchor peer update
+ res=0
+ set +x

#################################################################
#######    Generating anchor peer update for Org2MSP   ##########
#################################################################
+ configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org2MSPanchors.tx -channelID mychannel -asOrg Org2MSP
2019-10-01 22:53:57.710 -03 [common/tools/configtxgen] main -> INFO 001 Loading configuration
2019-10-01 22:53:57.722 -03 [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 002 Generating anchor peer update
2019-10-01 22:53:57.723 -03 [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 003 Writing anchor peer update
+ res=0
+ set +x

Creating network "net_byfn" with the default driver
Creating volume "net_peer0.org2.example.com" with default driver
Creating volume "net_peer1.org2.example.com" with default driver
Creating volume "net_peer1.org1.example.com" with default driver
Creating volume "net_peer0.org1.example.com" with default driver
Creating volume "net_orderer.example.com" with default driver
Creating peer1.org2.example.com ... 
Creating orderer.example.com ... 
Creating peer0.org2.example.com ... 
Creating peer1.org1.example.com ... 
Creating peer1.org2.example.com
Creating peer0.org1.example.com ... 
Creating orderer.example.com
Creating peer0.org2.example.com
Creating peer0.org1.example.com
Creating peer1.org2.example.com ... done
Creating cli ... 
Creating cli ... done

 ____    _____      _      ____    _____ 
/ ___|  |_   _|    / \    |  _ \  |_   _|
\___ \    | |     / _ \   | |_) |   | |  
 ___) |   | |    / ___ \  |  _ <    | |  
|____/    |_|   /_/   \_\ |_| \_\   |_|  

Build your first network (BYFN) end-to-end test

Channel name : mychannel
Creating channel...
+ peer channel create -o orderer.example.com:7050 -c mychannel -f ./channel-artifacts/channel.tx --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org1MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org1.example.com:7051
+ res=0
+ set +x
2019-10-02 01:54:02.624 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2019-10-02 01:54:02.645 UTC [channelCmd] InitCmdFactory -> INFO 002 Endorser and orderer connections initialized
2019-10-02 01:54:02.848 UTC [main] main -> INFO 003 Exiting.....
===================== Channel "mychannel" is created successfully ===================== 

Having all peers join the channel...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org1MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org1.example.com:7051
+ peer channel join -b mychannel.block
+ res=0
+ set +x
2019-10-02 01:54:02.891 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2019-10-02 01:54:02.961 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel
2019-10-02 01:54:02.961 UTC [main] main -> INFO 003 Exiting.....
===================== peer0.org1 joined on the channel "mychannel" ===================== 

CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org1MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer1.org1.example.com:7051
+ peer channel join -b mychannel.block
+ res=0
+ set +x
2019-10-02 01:54:06.029 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2019-10-02 01:54:06.114 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel
2019-10-02 01:54:06.115 UTC [main] main -> INFO 003 Exiting.....
===================== peer1.org1 joined on the channel "mychannel" ===================== 

CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org2MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org2.example.com:7051
+ peer channel join -b mychannel.block
+ res=0
+ set +x
2019-10-02 01:54:09.165 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2019-10-02 01:54:09.231 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel
2019-10-02 01:54:09.231 UTC [main] main -> INFO 003 Exiting.....
===================== peer0.org2 joined on the channel "mychannel" ===================== 

CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org2MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer1.org2.example.com:7051
+ peer channel join -b mychannel.block
+ res=0
+ set +x
2019-10-02 01:54:12.279 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2019-10-02 01:54:12.369 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel
2019-10-02 01:54:12.369 UTC [main] main -> INFO 003 Exiting.....
===================== peer1.org2 joined on the channel "mychannel" ===================== 

Updating anchor peers for org1...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org1MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org1.example.com:7051
+ peer channel update -o orderer.example.com:7050 -c mychannel -f ./channel-artifacts/Org1MSPanchors.tx --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
+ res=0
+ set +x
2019-10-02 01:54:15.440 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2019-10-02 01:54:15.447 UTC [channelCmd] update -> INFO 002 Successfully submitted channel update
2019-10-02 01:54:15.447 UTC [main] main -> INFO 003 Exiting.....
===================== Anchor peers for org "Org1MSP" on "mychannel" is updated successfully ===================== 

Updating anchor peers for org2...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org2MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org2.example.com:7051
+ peer channel update -o orderer.example.com:7050 -c mychannel -f ./channel-artifacts/Org2MSPanchors.tx --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
+ res=0
+ set +x
2019-10-02 01:54:18.516 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2019-10-02 01:54:18.522 UTC [channelCmd] update -> INFO 002 Successfully submitted channel update
2019-10-02 01:54:18.522 UTC [main] main -> INFO 003 Exiting.....
===================== Anchor peers for org "Org2MSP" on "mychannel" is updated successfully ===================== 

Installing chaincode on peer0.org1...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org1MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org1.example.com:7051
+ peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/
+ res=0
+ set +x
2019-10-02 01:54:21.605 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2019-10-02 01:54:21.605 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2019-10-02 01:54:21.679 UTC [main] main -> INFO 003 Exiting.....
===================== Chaincode is installed on peer0.org1 ===================== 

Install chaincode on peer0.org2...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org2MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org2.example.com:7051
+ peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/
+ res=0
+ set +x
2019-10-02 01:54:21.721 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2019-10-02 01:54:21.721 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2019-10-02 01:54:21.795 UTC [main] main -> INFO 003 Exiting.....
===================== Chaincode is installed on peer0.org2 ===================== 

Instantiating chaincode on peer0.org2...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org2MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org2.example.com:7051
+ peer chaincode instantiate -o orderer.example.com:7050 --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n mycc -l golang -v 1.0 -c '{"Args":["init","a","100","b","200"]}' -P 'OR	('\''Org1MSP.peer'\'','\''Org2MSP.peer'\'')'
+ res=0
+ set +x
2019-10-02 01:54:21.852 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2019-10-02 01:54:21.852 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2019-10-02 01:54:35.945 UTC [main] main -> INFO 003 Exiting.....
===================== Chaincode Instantiation on peer0.org2 on channel 'mychannel' is successful ===================== 

Querying chaincode on peer0.org1...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org1MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org1.example.com:7051
===================== Querying on peer0.org1 on channel 'mychannel'... ===================== 
Attempting to Query peer0.org1 ...3 secs
+ peer chaincode query -C mychannel -n mycc -c '{"Args":["query","a"]}'
+ res=0
+ set +x

2019-10-02 01:54:39.024 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2019-10-02 01:54:39.024 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
Query Result: 100
2019-10-02 01:54:52.533 UTC [main] main -> INFO 003 Exiting.....
===================== Query on peer0.org1 on channel 'mychannel' is successful ===================== 
Sending invoke transaction on peer0.org1...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org1MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer0.org1.example.com:7051
+ peer chaincode invoke -o orderer.example.com:7050 --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n mycc -c '{"Args":["invoke","a","b","10"]}'
+ res=0
+ set +x
2019-10-02 01:54:52.579 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2019-10-02 01:54:52.579 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2019-10-02 01:54:52.583 UTC [chaincodeCmd] chaincodeInvokeOrQuery -> INFO 003 Chaincode invoke successful. result: status:200 
2019-10-02 01:54:52.583 UTC [main] main -> INFO 004 Exiting.....
===================== Invoke transaction on peer0.org1 on channel 'mychannel' is successful ===================== 

Installing chaincode on peer1.org2...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org2MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer1.org2.example.com:7051
+ peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/
+ res=0
+ set +x
2019-10-02 01:54:52.617 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2019-10-02 01:54:52.617 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2019-10-02 01:54:52.691 UTC [main] main -> INFO 003 Exiting.....
===================== Chaincode is installed on peer1.org2 ===================== 

Querying chaincode on peer1.org2...
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
CORE_PEER_TLS_KEY_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.key
CORE_PEER_LOCALMSPID=Org2MSP
CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
CORE_PEER_TLS_CERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/server.crt
CORE_PEER_TLS_ENABLED=true
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
CORE_PEER_ID=cli
CORE_LOGGING_LEVEL=INFO
CORE_PEER_ADDRESS=peer1.org2.example.com:7051
===================== Querying on peer1.org2 on channel 'mychannel'... ===================== 
Attempting to Query peer1.org2 ...3 secs
+ peer chaincode query -C mychannel -n mycc -c '{"Args":["query","a"]}'
+ res=0
+ set +x

2019-10-02 01:54:55.764 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2019-10-02 01:54:55.764 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
Query Result: 90
2019-10-02 01:55:09.383 UTC [main] main -> INFO 003 Exiting.....
===================== Query on peer1.org2 on channel 'mychannel' is successful ===================== 

========= All GOOD, BYFN execution completed =========== 


 _____   _   _   ____   
| ____| | \ | | |  _ \  
|  _|   |  \| | | | | | 
| |___  | |\  | | |_| | 
|_____| |_| \_| |____/  


```

```bash
./bin/cryptogen generate --config=./crypto-config.yaml
org1.example.com
org2.example.com
```

Show, agora nos precisamos informar ao programa `configtxgen` para verificar no arquivo `configtx.yaml` o que ele deve ingerir (translate by google). 

```bash
export FABRIC_CFG_PATH=$PWD

sudo ../bin/configtxgen -profile TwoOrgsOrdererGenesis -channelID byfn-sys-channel -outputBlock ./channel-artifacts/genesis.block
2019-10-01 22:30:05.091 -03 [common/tools/configtxgen] main -> INFO 001 Loading configuration
2019-10-01 22:30:05.097 -03 [msp] getMspConfig -> INFO 002 Loading NodeOUs
2019-10-01 22:30:05.097 -03 [msp] getMspConfig -> INFO 003 Loading NodeOUs
2019-10-01 22:30:05.097 -03 [common/tools/configtxgen] doOutputBlock -> INFO 004 Generating genesis block
2019-10-01 22:30:05.098 -03 [common/tools/configtxgen] doOutputBlock -> INFO 005 Writing genesis block

```


Agora, precisamos criar o `channel transaction artifact`. Por favor, adicione a variabel de ambiente `$CHANNEL_NAME`.

```bash
export CHANNEL_NAME=mychannel

sudo ../bin/configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID $CHANNEL_NAME
2019-10-01 22:33:17.975 -03 [common/tools/configtxgen] main -> INFO 001 Loading configuration
2019-10-01 22:33:17.981 -03 [common/tools/configtxgen] doOutputChannelCreateTx -> INFO 002 Generating new channel configtx
2019-10-01 22:33:17.981 -03 [msp] getMspConfig -> INFO 003 Loading NodeOUs
2019-10-01 22:33:17.981 -03 [msp] getMspConfig -> INFO 004 Loading NodeOUs
2019-10-01 22:33:17.997 -03 [common/tools/configtxgen] doOutputChannelCreateTx -> INFO 005 Writing new channel tx
```


Agora, vamos definir uma `anchor` para o peer Org1:

```bash
sudo ../bin/configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/= -channelID $CHANNEL_NAME -asOrg Org1MSP
2019-10-01 22:36:45.399 -03 [common/tools/configtxgen] main -> INFO 001 Loading configuration
2019-10-01 22:36:45.404 -03 [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 002 Generating anchor peer update
2019-10-01 22:36:45.405 -03 [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 003 Writing anchor peer update

```
