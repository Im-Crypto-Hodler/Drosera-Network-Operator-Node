# Drosera-Network-Operator-Node


** 🪂🪂 Follow our X for More Early Alpha: [Twitter](https://x.com/imcryptohodler)**
---

# Recommended System Requirements
* 💻 2 CPU Cores
* 📊 4 GB RAM
* 🖳️ 20 GB Disk Space

## Buy VPS : [$5/Month](https://xorek.cloud/?from=25158)
---
  Open the link : [Xorex](https://xorek.cloud/?from=25158)  
  Select The VPS   
  Pay via $USDT -bep20  
  Wait for 5 Min  
  Activated ✅  


# Start Coding 👨‍💻

### ✏️ Open PuTTY 
Enter your IP from VPS and log in with PuTTY

```
root
```
 Enter password 


### 🚀 Install Dependencies
```
sudo apt-get update && sudo apt-get upgrade -y
```
```
sudo apt install curl ufw iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```

### 📦 Docker:
```bash
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=\"$(dpkg --print-architecture)\" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  \"$(. /etc/os-release && echo \"$VERSION_CODENAME\")\" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Test Docker
sudo docker run hello-world
```

<h1 align="center">🕵️ Trap Setup</h1>

## 1. ⚖️ Configure Environments
**Drosera CLI**:
```bash
curl -L https://app.drosera.io/install | bash
source /root/.bashrc
droseraup
```

**Foundry CLI**:
```bash
curl -L https://foundry.paradigm.xyz | bash
source /root/.bashrc
foundryup
```

**Bun:**
```bash
curl -fsSL https://bun.sh/install | bash
```

---

## 2. 👩‍🔧 Deploy Contract & Trap
```bash
mkdir my-drosera-trap && cd my-drosera-trap
```


```bash
git config --global user.email "Github_Email"
git config --global user.name "Github_Username"
```
* 🖐 Replace `Github_Email` & `Github_Username`

**Initialize Trap**:
```bash
forge init -t drosera-network/trap-foundry-template
```

**Compile Trap**:
```bash
bun install
forge build
```
> Skip warnings!

**☝ Deploy Trap**:
```bash
DROSERA_PRIVATE_KEY= <your_private_key> drosera apply
```
* 🖐 Replace `<your_private_key>` with your EVM wallet **private key** (Ensure it's funded with **Holesky ETH**)

When prompted, type `ofc` and press Enter.

---

## 3. 🔎 Check Trap in Dashboard
1. Connect your Drosera EVM wallet: https://app.drosera.io/
2. Click on `Traps Owned` to view deployed Traps or search by Trap address.

---

## 4. 🌿 Bloom Boost Trap
Go to your Trap on Dashboard and click `Send Bloom Boost`. Deposit some Holesky ETH ( Min. 2 ).

---

## 5. 🔄 Fetch Blocks
```bash
drosera dryrun
```

---

<h1 align="center">🚗 Operator Setup</h1>

## 1. 🔒 Whitelist Your Operator
```bash
cd my-drosera-trap
nano drosera.toml
```

Add the following:
```toml
private_trap = true
whitelist = ["your_operator_address"]
```
* 🖐 Replace `your_operator_address` with your EVM **public address** (same as your Metamask address).

```bash
DROSERA_PRIVATE_KEY= your_private_key drosera apply
```
* 🖐 Replace `your_private_key` with your EVM wallet **private key**

---

## 2. 💾 Operator CLI
```bash
cd ~
curl -LO https://github.com/drosera-network/releases/releases/download/v1.16.2/drosera-operator-v1.16.2-x86_64-unknown-linux-gnu.tar.gz
tar -xvf drosera-operator-v1.16.2-x86_64-unknown-linux-gnu.tar.gz
sudo cp drosera-operator /usr/bin
```
Test:
```bash
drosera-operator --version
drosera-operator
```

---

## 3. 📂 Install Docker Image
```bash
docker pull ghcr.io/drosera-network/drosera-operator:latest
```

---

## 4. 🚫 Register Operator
```bash
drosera-operator register --eth-rpc-url https://ethereum-holesky-rpc.publicnode.com --eth-private-key your_private_key
```
* 🖐 Replace `your_private_key` with your **Metamask wallet private key**

---

## 5. 🔓 Open Ports
```bash
sudo ufw allow ssh
sudo ufw allow 22
sudo ufw enable

sudo ufw allow 31313/tcp
sudo ufw allow 31314/tcp
```

---

## 6. 🌐 Install & Run Operator

### 🔢 Method 2: SystemD (Recommended First)
#### 🔍 6-1: Configure SystemD
```bash
sudo tee /etc/systemd/system/drosera.service > /dev/null <<EOF
[Unit]
Description=drosera node service
After=network-online.target

[Service]
User=$USER
Restart=always
RestartSec=15
LimitNOFILE=65535
ExecStart=$(which drosera-operator) node --db-file-path $HOME/.drosera.db --network-p2p-port 31313 --server-port 31314 \
    --eth-rpc-url https://ethereum-holesky-rpc.publicnode.com \
    --eth-backup-rpc-url https://1rpc.io/holesky \
    --drosera-address 0xea08f7d533C2b9A62F40D5326214f39a8E3A32F8 \
    --eth-private-key your_private_key \
    --listen-address 0.0.0.0 \
    --network-external-p2p-address your_vps_ip \
    --disable-dnr-confirmation true

[Install]
WantedBy=multi-user.target
EOF
```
* 🖐 Replace `your_private_key` with your EVM wallet **private key**
* 🖐 Replace `your_vps_ip` with your **VPS IP**

#### 🛠️ 6-2: Start SystemD
```bash
sudo systemctl daemon-reload
sudo systemctl enable drosera
sudo systemctl start drosera
```

#### 🔌 6-3: Check Node Health
```bash
journalctl -u drosera.service -f
```

---

### 🔢 Method 1: Docker
#### 🌐 6-1: Configure Docker
```bash
git clone https://github.com/0xmoei/Drosera-Network
cd Drosera-Network
cp .env.example .env
nano .env
```
* 🖐 Replace `your_evm_private_key` with `your_private_key`
* 🖐 Replace `your_vps_public_ip` with `your_vps_ip`

#### 🚀 6-2: Run Operator
```bash
docker compose up -d
```

#### 🔎 6-3: Check health
```bash
docker compose logs -f
```

#### ⚠️ Optional Docker Commands
```bash
# Stop
docker compose down -v
# Restart
docker compose up -d
```

---

## 7. 🛋️ Opt-in Trap
Dashboard > Open your Trap > Click `Opt-in` to connect operator.

---

## 8. 🚗 Node Liveness
Check dashboard: you should see 🟢 **green blocks** showing node activity.

---
🚀 You're all set! Enjoy farming with Drosera!
