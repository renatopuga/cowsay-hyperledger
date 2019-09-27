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
