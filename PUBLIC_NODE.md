# Public node setup guide
`gm` can be used to set up nodes connecting to existing networks.
Below is an example connecting to the Cosmos Hub from an Intel Mac.
(You can customize it to other operating systems by changing the binary in `gm.toml`.)

Useful links:
* [Chain registry](https://github.com/cosmos/chain-registry)
* [Gaiad release v7.1.0](https://github.com/cosmos/gaia/releases/tag/v7.1.0)

## Set up `gm`
Minimalistic gm.toml:
```toml
[global]
gaiad_binary = "https://github.com/cosmos/gaia/releases/download/v7.1.0/gaiad-v7.1.0-darwin-amd64"
[cosmoshub-4]
```
One-node, no-nonsense Gaiad testnet network.

## Set up the folders
We `gm start` to create the folders for this local testnet called "cosmoshub-4"
and then we `gm stop` it, so we can replace the configuration with the mainnet data.
We do a `gm reset` to remove the testnet database entries.
```bash
$ gm start
Creating cosmoshub-4 config...
cosmoshub-4 started, PID: 97569, LOG: /Users/maphlaves/.gm/cosmoshub-4/log
$ gm stop
Stopping cosmoshub-4 with PID 97569...
$ gm reset cosmoshub-4
Resetting cosmoshub-4...
I[2022-12-08|11:47:06.989] Removed existing address book                file=/Users/maphlaves/.gm/cosmoshub-4/config/addrbook.json
I[2022-12-08|11:47:06.994] Removed all blockchain history               dir=/Users/maphlaves/.gm/cosmoshub-4/data
I[2022-12-08|11:47:06.996] Reset private validator file to genesis state keyFile=/Users/maphlaves/.gm/cosmoshub-4/config/priv_validator_key.json stateFile=/Users/maphlaves/.gm/cosmoshub-4/data/priv_validator_state.json
```

## Configure the node
We are ready to configure our node. Download the Cosmos Hub genesis from the chain registry:
```bash
$ wget -O ~/.gm/cosmoshub-4/config/genesis.json.gz https://github.com/cosmos/mainnet/raw/master/genesis/genesis.cosmoshub-4.json.gz
#(modem noises)
$ gunzip ~/.gm/cosmoshub-4/config/genesis.json.gz
/Users/maphlaves/.gm/cosmoshub-4/config/genesis.json already exists -- do you wish to overwrite (y or n)? y
```

Now onto the more complex task of changing our node configuration parameters:
Edit your `app.toml` at `$HOME/.gm/cosmoshub-4/config/app.toml` and change:
```toml
minimum-gas-prices = "0.002uatom"
```
This ensures a minimal amount of spam protection: zero-fee transactions will be rejected from the mempool.

Edit your `config.toml` at `$HOME/.gm/cosmoshub-4/config/config.toml` and change:
```toml
moniker = "anything you like"
[p2p]
addr_book_strict = true
allow_duplicate_ip = false
external_address = "find out your public IP address:27003"
persistent_peers = "ee27245d88c632a556cf72cc7f3587380c09b469@45.79.249.253:26656,538ebe0086f0f5e9ca922dae0462cc87e22f0a50@34.122.34.67:26656,d3209b9f88eec64f10555a11ecbf797bb0fa29f4@34.125.169.233:26656,bdc2c3d410ca7731411b7e46a252012323fbbf37@34.83.209.166:26656,585794737e6b318957088e645e17c0669f3b11fc@54.160.123.34:26656,11dfe200894f38e411beca77928e9dd118e66813@94.130.98.157:26656,5b4ed476e01c49b23851258d867cc0cfc0c10e58@206.189.4.227:26656,654f47a762c8f9257aef4a44c1fb5014916d8b20@99.79.60.15:26656,366ac852255c3ac8de17e11ae9ec814b8c68bddb@51.15.94.196:26656,d6318b3bd51a5e2b8ed08f2e520d50289ed32bf1@52.79.43.100:26656,1bfda3d59e70290a3dada9bb809dd954371850d3@54.180.225.240:26656,6ee94c2093505e8790442c054e6e1e0211d36583@44.239.140.195:26656,ec779a2741da6dd2ccdaa6dfc0bebb10e595dfa4@50.18.113.67:26656,cfd785a4224c7940e9a10f6c1ab24c343e923bec@164.68.107.188:26656,d72b3011ed46d783e369fdf8ae2055b99a1e5074@173.249.50.25:26656,047f723806ee702b211e7227f89eacd829aabd86@52.9.212.125:26656,b0e746acb6fbed7a0311fe21cfb2ee94581ca3bc@51.79.21.187:26656,82772547c4575c18dfe6e75aafe521cf7d4dc8de@142.93.157.186:26656,3c7cad4154967a294b3ba1cc752e40e8779640ad@84.201.128.115:26656,f122129f53b7c584df6cee77716dcc636d5c5e18@167.172.59.196:26656,241b17dba97a2ed3c3747d12781fb86c9706e2d4@95.179.136.131:26656,f1b16c603f3a0e59f0ce5179dc80f549a7ecd0e2@sentries.us-east1.iqext.net:26656,64bd8eaf08b05f17ccd88425f80b59ab48934004@157.90.18.35:26656,1da54d20c7339713f1d6d28dd2117087dd33d0ca@cosmos-seed.icycro.org:26656"
seeds = "bf8328b66dceb4987e5cd94430af66045e59899f@public-seed.cosmos.vitwit.com:26656,cfd785a4224c7940e9a10f6c1ab24c343e923bec@164.68.107.188:26656,d72b3011ed46d783e369fdf8ae2055b99a1e5074@173.249.50.25:26656,ba3bacc714817218562f743178228f23678b2873@public-seed-node.cosmoshub.certus.one:26656,3c7cad4154967a294b3ba1cc752e40e8779640ad@84.201.128.115:26656,366ac852255c3ac8de17e11ae9ec814b8c68bddb@51.15.94.196:26656,bcef90de8a83673c336bf3b3a352445b3a3a1f08@cosmos-seed.sunshinevalidation.io:31038,3b67739570f921cc5e0db4b3efe488ce184155a9@seeds.pupmos.network:2000,ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@seeds.polkachu.com:14956,20e1000e88125698264454a884812746c2eb4807@seeds.lavenderfive.com:14956,57a5297537b9b6ef8b105c08a8ad3f6ac452c423@seeds.goldenratiostaking.net:1618"
```
The first two settings will require other nodes to be on the public Internet. This was not a requirement on testnets.

To find your public IP address, you can run `curl api.ipify.org` in the terminal.

You can get a fresh list of persistent_peers and/or seeds from the [chain registry](https://github.com/cosmos/chain-registry/blob/master/cosmoshub/chain.json).

You do not need to populate both `seeds` and `persistent_peers` but you need to populate at least one.

If you are behind a firewall you don't control, setting the external IP address might not really help:
no one will be able to connect to your node unless the p2p port (27003 in the example) is routed toward your machine.
You will be able to connect to other nodes, though, just the initial startup might be slower.

## State syncing
State syncing requires the following information:
* A hash for a trusted, previous height
* Two RPC nodes for some authentication of that information
* P2P connections so the node can gather the state snapshot

Your node will go and search for state snapshots over P2P that were created above "trusted height".

Set these in config.toml:
```toml
[statesync]
enable = true
rpc_servers = "https://cosmos-rpc.polkachu.com:443,https://rpc-cosmoshub.blockapsis.com:443"
trust_hash = "3E8009B4D306ADD7CB19AE1ACEAD882F0A2D4F5E0418910989F194788E227539"
trust_height = 13165039
```
You can find RPC servers in the [chain registry](https://github.com/cosmos/chain-registry/blob/master/cosmoshub/chain.json).

Set the trust hash and trust height by looking at an existing, trusted Gaia node and query some older heights from it.

## Start your node
...and see how it behaves:
```bash
gm start
gm log cosmoshub-4 -f
```

The node will start with invariant checks then it will try to dial some peers. After a while it will start stake syncing
and you will see similar entries in the log:
```
1:37PM INF Fetching snapshot chunk chunk=43 format=1 height=13166000 module=statesync total=76
```

If you poll the `/status` endpoint, you will see that the node is catching up:
```json
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "node_info": {},
    "sync_info": {
      "latest_block_hash": "",
      "latest_app_hash": "",
      "latest_block_height": "0",
      "latest_block_time": "1970-01-01T00:00:00Z",
      "earliest_block_hash": "",
      "earliest_app_hash": "",
      "earliest_block_height": "0",
      "earliest_block_time": "1970-01-01T00:00:00Z",
      "catching_up": true
    },
    "validator_info": {}
  }
}
```

Then your laptop overheats.

## System requirements
Finally, we should mention what you always find out last: how much resource will a node need?
The current Cosmos Hub node in production uses about 6-8 cores, 30GB of RAM and 300GB of disk space allocated.
You can try with lower settings as a test but depending on network, some require a lot of RAM for crisis invariant checks.
The good news is that if you run this on your laptop, it will eat up all your available resources.

`gm` is flexible, so you can even start your node directly with gaiad (but it's worth doing it through `gm` for convenience).

To start a node with no invariant checks, you run:
```bash
gaiad start --x-crisis-skip-assert-invariants --home $HOME/.gm/cosmoshub-4
```
In this case, managing the process is completely out of `gm`'s hands.
