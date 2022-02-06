## Installing Cardano Node and CLI  
The follwoing steps are for an Ubuntu 20.04 on a Virtual box, for any other system combination, commands can be slightly different. You can visit below link for further information.  
https://developers.cardano.org/docs/get-started/installing-cardano-node

1.  Open the ternimal and run below commands.  
    sudo apt-get update -y && sudo apt-get upgrade -y    
    sudo apt-get install automake build-essential pkg-config libffi-dev libgmp-dev libssl-dev libtinfo-dev libsystemd-dev zlib1g-dev make g++ tmux git jq wget libncursesw5 libtool autoconf -y     
    sudo apt install curl  
    curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh  
		Press enter for each instruction after running the above cmd.  
2.  Restart your teminal and run below commands  
    ghcup install ghc 8.10.7  
    ghcup set ghc 8.10.7 
    ghcup install cabal 3.6.2.0  
    ghcup set cabal 3.6.2.0  
    ghc --version  
    cabal --version  
    mkdir -p $HOME/cardano-src  
    cd $HOME/cardano-src  
    git clone https://github.com/input-output-hk/libsodium  
    cd libsodium  
    git checkout 66f017f1  
    ./autogen.sh  
    ./configure  
    make  
    sudo make install  
3.  Move to the home directory and run   
    nano .bashrc  
4.  Edit the file by adding the following line at the end of the file.  
	export LD_LIBRARY_PATH="/usr/local/lib:$LD_LIBRARY_PATH"  
	export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH"  
5.  Restart the terminal and run below commands  
    cd $HOME/cardano-src  
    git clone https://github.com/input-output-hk/cardano-node.git  
    cd cardano-node  
    git fetch --all --recurse-submodules --tags  
    sudo apt install jq  
    git checkout $(curl -s https://api.github.com/repos/input-output-hk/cardano-node/releases/latest | jq -r .tag_name)  
    cabal configure --with-compiler=ghc-8.10.7  
6.  If you are running non x86/x64 platform (eg. ARM) please install and configure LLVM with:  
    sudo apt install llvm-9  
    sudo apt install clang-9 libnuma-dev  
    sudo ln -s /usr/bin/llvm-config-9 /usr/bin/llvm-config  
    sudo ln -s /usr/bin/opt-9 /usr/bin/opt  
    sudo ln -s /usr/bin/llc-9 /usr/bin/llc  
    sudo ln -s /usr/bin/clang-9 /usr/bin/clang  
7.  Run below commands  
    sudo apt install libgmp-dev  
    cabal build cardano-node cardano-cli  
    mkdir -p $HOME/.local/bin  
    cp -p "$(./scripts/bin-path.sh cardano-node)" $HOME/.local/bin/  
    cp -p "$(./scripts/bin-path.sh cardano-cli)" $HOME/.local/bin/  
    export PATH="$HOME/.local/bin/:$PATH"  
8.  Restart the terminal and confirm the installation with below commands  
    cardano-cli --version  
    cardano-node --version  

## Running Cardano Node
1.  Below are the commands to fetch the config files for the mainnet and testnet, Do run commands to have a copy of the     config files.   
Network Magic : 764824073   
    curl -O -J https://hydra.iohk.io/build/7370192/download/1/mainnet-config.json  
    curl -O -J https://hydra.iohk.io/build/7370192/download/1/mainnet-byron-genesis.json  
    curl -O -J https://hydra.iohk.io/build/7370192/download/1/mainnet-shelley-genesis.json  
    curl -O -J https://hydra.iohk.io/build/7370192/download/1/mainnet-alonzo-genesis.json  
    curl -O -J https://hydra.iohk.io/build/7370192/download/1/mainnet-topology.json  

NetworkMagic: 1097911063  
    curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-topology.json  
    curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-shelley-genesis.json  
    curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-config.json  
    curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-byron-genesis.json  
    curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-alonzo-genesis.json  

2.  You can place above config files in a separate folder.  
3.  Also make a directory for db inside cardano-node, amke sure to double check the address location of your config files and the update accordingly in the below command.   
    cardano-node run \  
    --topology $HOME/cardano-src/cardano-node/config-files/testnet-topology.json \  
    --database-path $HOME/cardano-src/cardano-node/db \  
    --socket-path $HOME/cardano-src/cardano-node/db/node.socket \  
    --host-addr 127.0.0.1 \  
    --port 3001 \  
    --config $HOME/cardano-src/cardano-node/config-files/testnet-config.json   
4.  Above command will run the Cardano node.   

## Querying  

1.  Open .bashrc file and add the below line at the end.  
    export CARDANO_NODE_SOCKET_PATH$HOME/cardano-src/cardano-node/db/node.socket "  
2.  Restart your terminal  
    cd home/cardano-src/cardano-node/  
	cardano-cli query tip --testnet-magic 1097911063  
