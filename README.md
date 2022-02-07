
# :crystal_ball: Run Cardano test node
- Linux environment required
- Installing prerequisites
```bash
sudo apt-get update -y && sudo apt-get upgrade -y
sudo apt-get install automake build-essential pkg-config libffi-dev libgmp-dev libssl-dev libtinfo-dev libsystemd-dev zlib1g-dev make g++ tmux git jq wget libncursesw5 libtool autoconf -y
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
ghcup install ghc 8.10.7
ghcup set ghc 8.10.7
ghcup install cabal 3.6.2.0
ghcup set cabal 3.6.2.0
```



- downloading and compiling
```bash
mkdir -p $HOME/cardano-src
cd $HOME/cardano-src
git clone https://github.com/input-output-hk/libsodium
cd libsodium
git checkout 66f017f1
./autogen.sh
./configure
make
sudo make install

export LD_LIBRARY_PATH="/usr/local/lib:$LD_LIBRARY_PATH"
export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH"

cd $HOME/cardano-src
git clone https://github.com/input-output-hk/cardano-node.git
cd cardano-node
git fetch --all --recurse-submodules --tags
git checkout $(curl -s https://api.github.com/repos/input-output-hk/cardano-node/releases/latest | jq -r .tag_name)
```



- configuring and build
```bash
cabal configure --with-compiler=ghc-8.10.7
sudo apt install llvm-9
sudo apt install clang-9 libnuma-dev
sudo ln -s /usr/bin/llvm-config-9 /usr/bin/llvm-config
sudo ln -s /usr/bin/opt-9 /usr/bin/opt
sudo ln -s /usr/bin/llc-9 /usr/bin/llc
sudo ln -s /usr/bin/clang-9 /usr/bin/clang
cabal build cardano-node cardano-cli
mkdir -p $HOME/.local/bin
cp -p "$(./scripts/bin-path.sh cardano-node)" $HOME/.local/bin/
cp -p "$(./scripts/bin-path.sh cardano-cli)" $HOME/.local/bin/
export PATH="$HOME/.local/bin/:$PATH"
```



- checking the installation
```
cardano-cli --version
cardano-node --version
```



- run the testnet node
   - make a new folder in $HOME named 'cardano'
   - make a new folder inside $HOME/cardano named 'db'
   - cd to $HOME/cardano
   - run the code below in terminal to download the testnet configuration files:
```
curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-topology.json
curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-shelley-genesis.json
curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-config.json
curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-byron-genesis.json
curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-alonzo-genesis.json
```
- run the following code in terminal to run the node:
```
cardano-node run \
   --topology $HOME/cardano/testnet-topology.json \
   --database-path $HOME/cardano/db \
   --socket-path $HOME/cardano/db/node.socket \
   --host-addr 127.0.0.1 \
   --port 3001 \
   --config $HOME/cardano/testnet-config.json
```



# :snowflake: Run Fluree
- install Java
- download Fluree from https://developers.flur.ee/docs/overview/getting_started/#install-fluree
- run in terminal:
```
./fluree_start.sh
```
- the cli will show the address and port admin GUI is running on


# :milky_way: Metaverses
- follow the steps here https://www.reddit.com/r/decentraland/comments/m0xujc/decentralizing_decentraland_content_setting_up_a/
