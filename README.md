# Layeredge-VPS-Node-Guide
Layeredge VPS Node Guide

This is a guide on how to run the Layer Edge light node on your VPS server! After many hours of receiving errors, I have found the easiest way it works
Unlike other installations, in this guide I used a specific version of GO (1.21.6) because with other versions I encountered some errors when running the node.
Without further ado, here are the steps.

1️⃣Initial Setup

Open your VPS terminal and run the following commands:

```sh
sudo apt update && sudo apt upgrade -y  # Updates the package list and upgrades installed packages to their latest versions.
sudo apt install build-essential        # Installs essential tools for compiling software, like compilers and libraries.
sudo apt install git -y                # Installs Git, a version control tool for managing code.
sudo apt install screen -y             # Installs Screen, a utility for running processes in the background, even after closing the terminal.
```

2️⃣ Start a Screen Session to Keep Your Node Active

Use this command to ensure your node stays running even if you log out or close the terminal:

```sh
screen -S layeredge
```
3️⃣ Clone Light Node Repository

```sh
git clone https://github.com/Layer-Edge/light-node.git

```
then

```sh
cd light-node

```
4️⃣ Install Dependencies

Install the following tools and dependencies:

Install GO

This command downloads and installs a specific version of Go (1.21.6) that I have personally verified to work, as other versions might cause issues when running the node

```sh
wget https://go.dev/dl/go1.21.6.linux-amd64.tar.gz; sudo tar -C /usr/local -xzf go1.21.6.linux-amd64.tar.gz; echo "export GOROOT=/usr/local/go" >> ~/.bashrc; echo "export GOPATH=\$HOME/go" >> ~/.bashrc; echo "export PATH=\$GOPATH/bin:\$GOROOT/bin:\$PATH" >> ~/.bashrc; source ~/.bashrc; go version
```
Install Rust
```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
```
Install Risc0 Toolchain
```sh
curl -L https://risczero.com/install | bash
```

```sh
source "/root/.bashrc"

```

```sh
rzup install
```

```sh
source "/root/.bashrc"

```
Install Cargo
```sh
sudo apt install cargo -y

```
5️⃣ Configure .env file 

Open the Nano text editor to create a new .env file:
```sh
nano .env

```
and paste:
You need a EVM wallet private key for this step, you can use a burner wallet.
```sh
GRPC_URL=grpc.testnet.layeredge.io:9090
CONTRACT_ADDR=cosmos1ufs3tlq4umljk0qfe8k5ya0x6hpavn897u2cnf9k0en9jr7qarqqt56709
ZK_PROVER_URL=http://127.0.0.1:3001
# Alternatively:
ZK_PROVER_URL=http://layeredge.mintair.xyz
API_REQUEST_TIMEOUT=100
POINTS_API=http://light-node.layeredge.io
PRIVATE_KEY=your evm private key here, you can use BURNER Wallet!!!
```
Press CTRL X,then Y, and Enter to save.

6️⃣ Run Merkle Service

```sh
cd risc0-merkle-service
cargo build && cargo run

```
Once the process finishes and you see "Starting server on port 3001...", press CTRL + A, then D to detach from the screen session.

7️⃣ Build Light Node

Open another screen by running:
```sh
screen -S light-node
```
Change to the Light Node directory:

```sh
cd light-node

```
Build the Light Node:
```sh
go build
```
8️⃣ Run Light Node

Start the Light Node:

```sh
./light-node

```
A public key will be displayed on your screen. Make sure to copy and store it, as it is required to connect your CLI Node to the dashboard.
If you see in your logs: "Worker 1 is running", you are done whit the node

9️⃣ Link your node

​To link your CLI node with the LayerEdge dashboard:​

🌐 Visit https://dashboard.layeredge.io/.​

🔗 Connect your wallet

📊 In the dashboard, locate "CLI Node Points".​

➕ Click the "+" button.​

🔑 Paste the public key obtained when running your node.​

✅ Done!​

Currently, the linking process may experience some issues, but these are expected to be resolved soon.
