# eth2launch - ETH2.0 post merge node operator launch tamplate

## Background
### The Merge and the Stucked Node
In Jun/Jul 2022, ETH2.0 has been merged progressively. Official explain: [The Merge](https://ethereum.org/en/upgrades/merge/)

Node operators have to made some changes on the way of hosting node, otherwise we'll find that the full node syncing is getting slow and finally stucked at a certain height (some day in the early Jul).
The error msg could be like this:
```
Local chain is post-merge, waiting for beacon client sync switch-over
```
An related issue can be found [here](https://github.com/ethereum/go-ethereum/issues/25118)

### How to Run Full Node After the Merge
According to [Ethereum's doc](https://ethereum.org/en/developers/docs/nodes-and-clients/run-a-node/):

In ETH2.0 post-merge, the full node has been splited into 2 clients: Execution Layer client (EL) and Consensus Layer client (CL), one must run bot EL&CL clients in order to operate a full node.

The classic eth node 'geth' that we familiared with becomes EL only, so we need to choice and run a CL client and connect it to EL client.

The mainstream EL / CL clients can be found [here](https://ethereum.org/en/upgrades/get-involved/#clients)

Although the ethereum core team is encouraging the community to [choice 'minority client'](https://ethereum.org/en/developers/docs/nodes-and-clients/client-diversity/#use-minority-client), to increase the decentralization and diversity, here we only take geth+lighthouse docker compose file as an example.

Note: the following Instructions is for sepolia testnet, which is also merged in the early July.

## Instructions
1. genereate jwt token for engine API (port 8551 by default)
```
$ ./gen_jwt.sh > "jwt.hex"
```

2. modify your geth node ip in docker-compose.yml
- currently there's an issue using docker service name, so have to use the host ip or the public ip as an work around
in `docker-compose.yml`, replace `<geth_ip>` with the actual geth ip
```
- --eth1-endpoints=http://<geth_ip>:8545
- --execution-endpoints=http://<geth_ip>:8551
```

3. run docker compose
```
$ docker-compose up -d
```

4. check docker logs:
```
$ watch -n 1 docker logs -n 20 watch -n 1 docker logs -n 20 eth2launch_geth_1
```
```
$ watch -n 1 docker logs -n 20 watch -n 1 docker logs -n 20 eth2launch_lighthouse_1
```

## Ref
- https://ethereum.org/en/upgrades/merge/
- https://notes.ethereum.org/@launchpad/kiln
- https://ethereum.org/en/upgrades/get-involved/#clients
- https://ethereum.org/en/developers/docs/nodes-and-clients/run-a-node/
- https://lighthouse-book.sigmaprime.io/intro.html
- https://blog.ethereum.org/2022/06/30/sepolia-merge-announcement/
- https://github.com/ethereum/execution-apis/blob/main/src/engine/specification.md
- https://github.com/ethereum/execution-apis/blob/main/src/engine/authentication.md
- https://github.com/eth-clients/sepolia
- https://github.com/ethereum/go-ethereum/issues/25118


## Author
@xhyumiracle
@iczc
