# Simple Voting Smart Contract

This repository provides a comprehensive guide to creating, building, deploying, and interacting with a simple voting smart contract on the **MultiversX blockchain**.

---

## Why MultiversX?

MultiversX is a high-performance blockchain designed for scalability, efficiency, and low-cost transactions, making it ideal for smart contract development. Here's why:

### **Performance**
- **High Throughput:** Handles up to 15,000 transactions per second (TPS) with transaction costs as low as $0.001.
- **Scalability:** Adaptive State Sharding enables the network to scale beyond 100,000 TPS as it grows.
- **Tested Efficiency:** Reached up to 263,000 TPS on the testnet.

### **Developer Ecosystem**
- **Comprehensive Tools:** Includes an IDE, a Rust-based framework, and debugging utilities.
- **Incentives:** Developers earn 30% of gas fees for contract execution.
- **WASM Compatibility:** Allows the use of WebAssembly (WASM) for diverse programming languages.

### **Rust Framework**
Rust offers a robust environment for developing efficient and secure smart contracts with minimal gas costs.

---

## **Getting Started**

### **Step 1: Install `pipx`**
`pipx` is the recommended tool for installing the MultiversX CLI (`mxpy`).

#### Ubuntu & Windows WSL
```bash
sudo apt update
sudo apt install pipx
pipx ensurepath
```

#### macOS
```bash
brew install pipx
pipx ensurepath
```

Verify installation:
```bash
pipx --version
```

### **Step 2: Install `mxpy`**
Install the MultiversX CLI for blockchain interactions and smart contract management:
```bash
pipx install multiversx-sdk-cli --force
```

### **Step 3: Install Rust**
Rust is required to compile smart contracts.

#### Ubuntu & Windows WSL
```bash
sudo apt install build-essential pkg-config libssl-dev
```

#### macOS
```bash
xcode-select --install
```

Install Rust via `mxpy`:
```bash
mxpy deps install rust --overwrite
```

Verify installation:
```bash
mxpy --version
rustup show
```

---

## Creating a New Smart Contract Project

Initialize a new smart contract project:
```bash
sc-meta new --template empty --name simple-voting
```

Verify the project setup:
```bash
cargo check
```

---

## Building the Smart Contract

Compile the smart contract into WebAssembly (WASM) bytecode:
```bash
sc-meta all build
```

This generates an output directory containing:
```
output/
├── simple-voting.abi.json
├── simple-voting.imports.json
├── simple-voting.mxsc.json
└── simple-voting.wasm
```
#### Key Outputs:
- **`simple-voting.wasm`**: Compiled bytecode for deployment.
- **`simple-voting.abi.json`**: ABI for interacting with the contract.

---

## Deploying the Smart Contract

### Deployment to the Devnet

Define the deployment arguments in `deploy_arguments.json`:
```json
[
  "What is your favorite programming language?", 
  ["Rust", "Python", "JavaScript", "C++"]
]
```

Deploy the contract:
```bash
mxpy --verbose contract deploy \
  --recall-nonce \
  --bytecode="./output/simple-voting.wasm" \
  --proxy=https://devnet-gateway.multiversx.com \
  --abi ./output/simple-voting.abi.json \
  --arguments-file ./deploy_arguments.json \
  --gas-limit 500000000 \
  --keyfile="./<your-wallet-keyfile>.json" \
  --send
```

Replace `<your-wallet-keyfile>.json` with your wallet keyfile name. Note the contract address after successful deployment.

---

## Upgrading the Smart Contract

To upgrade a deployed contract:

1. Build the updated contract:
   ```bash
   sc-meta all build
   ```

2. Deploy the upgraded bytecode:
   ```bash
   mxpy contract upgrade erd1<your-contract-address> \
     --bytecode ./output/simple-voting.wasm \
     --proxy=https://devnet-gateway.multiversx.com \
     --chain D \
     --recall-nonce \
     --gas-limit 5000000 \
     --keyfile="./<your-wallet-keyfile>.json" \
     --send
   ```

---

## Interacting with the Smart Contract

### Querying the Contract
Example: Fetch the current poll question.
```bash
mxpy contract query erd1<your-contract-address> \
  --proxy https://devnet-gateway.multiversx.com \
  --function getPollQuestion
```

### Voting Example
Cast a vote for an option (e.g., `Rust`):
```bash
mxpy contract call erd1<your-contract-address> \
  --function vote \
  --arguments str:Rust \
  --proxy https://devnet-gateway.multiversx.com \
  --keyfile "./<your-wallet-keyfile>.json" \
  --send
```

---

## Best Practices
1. **Secure Your Wallet:** Keep your wallet keyfile safe and backed up.
2. **Optimize Gas Usage:** Write efficient smart contract code to reduce gas costs.
3. **Test Thoroughly:** Test your contract extensively on the devnet before deploying to the mainnet.


