 # 1. Server Requirements
Component	Requirements
CPU	8
Storage	500 GB
Ram	16 GB
Port	30333 - P2P
9933 - RPC
9615 - Prometh
OS	Ubuntu 22.04
# 2. Update & install the necessary utilities
### Change Your-name
```
MONIKER=VNBnode
```
```
apt update && apt upgrade -y
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev libgmp3-dev tar clang bsdmainutils ncdu unzip llvm libudev-dev make protobuf-compiler -y
```

# 3. Download binaries files
```
mkdir -p $HOME/.tangle && cd $HOME/.tangle
wget -O tangle https://github.com/webb-tools/tangle/releases/download/v1.0.1-rc1/tangle-testnet-linux-amd64

chmod 744 tangle
mv tangle /usr/bin/
tangle --version
```

### **Result should be**: tangle 1.0.0-6855ead-x86_64-linux-gnu

# 4. create tangle service

```
sudo tee /etc/systemd/system/tangle.service > /dev/null << EOF
[Unit]
Description=Tangle Validator Node
After=network-online.target
StartLimitIntervalSec=0
[Service]
User=$USER
Restart=always
RestartSec=3
LimitNOFILE=65535
ExecStart=/usr/bin/tangle \
  --base-path $HOME/.tangle/data/ \
  --name $MONIKER \
  --validator \
  --chain tangle-testnet \
  --node-key-file "$HOME/.tangle/node-key" \
  --port 9946 \
  --telemetry-url "wss://telemetry.polkadot.io/submit 1" \
  --no-mdns
[Install]
WantedBy=multi-user.target
EOF
```
```
systemctl daemon-reload
systemctl enable tangle
```
# 5. Add keys

**Account keys
Acco**
```
tangle key insert --base-path $HOME/.tangle/data/ \
--chain tangle-testnet \
--scheme Sr25519 \
--suri "banana puzzle warfare shell gate enroll flash middle early assault napkin agent" \
--key-type acco
```
**Babe Keys
Babe**
 ```
tangle key insert --base-path $HOME/.tangle/data/ \
--chain tangle-testnet \
--scheme Sr25519 \
--suri "banana puzzle warfare shell gate enroll flash middle early assault napkin agent" \
--key-type babe
```
**Im-online Keys - these keys are optional
Imonline**
```
tangle key insert --base-path $HOME/.tangle/data/ \
--chain tangle-testnet \
--scheme Sr25519 \
--suri "banana puzzle warfare shell gate enroll flash middle early assault napkin agent" \
--key-type imon
```
**Role Keys
Role**
```
tangle key insert --base-path $HOME/.tangle/data/ \
--chain tangle-testnet \
--scheme Sr25519 \
--suri "banana puzzle warfare shell gate enroll flash middle early assault napkin agent" \
--key-type role
```
**Grandpa Keys
Grandpa**
```
tangle key insert --base-path $HOME/.tangle/data/ \
--chain tangle-testnet \
--scheme Sr25519 \
--suri "banana puzzle warfare shell gate enroll flash middle early assault napkin agent" \
--key-type gran
```
**Check keystore**
```
cd $Home
ls .tangle/data/chains/tangle-testnet/keystore
```
**6. Run service & check logs**
```
systemctl restart tangle && journalctl -u tangle -f -o cat
```
**7. get your session key**
```
curl -H "Content-Type: application/json" -d '{ "id": 1, "jsonrpc": "2.0", "method": "author_rotateKeys", "params": [] }' http://localhost:9944
```
**8. Telemetry:**

https://telemetry.polkadot.io/#list/0x624a09c9b7446ecd3ef5a00fb6a0554be1cd6e4472da050bd45a2a61c62d96c2

**9. Explorer**

https://polkadot.js.org/apps/?rpc=wss://testnet-rpc.tangle.tools#/explorer

