# Simple Voting

---

## Why MultiversX?

MultiversX offers a robust blockchain platform with unparalleled performance and developer-friendly tools. Here's why you should consider it:

### **Performance**
- **Throughput**: 15,000 transactions per second (TPS) with $0.001 transaction cost.
- **Scalability**: Adaptive State Sharding allows scaling beyond 100,000 TPS as the network grows.
- **Efficiency**: Achieved 263,000 TPS on testnet.

### **Developer Ecosystem**
- **IDE and Framework**: MultiversX IDE, Rust-based framework, and debugging tools.
- **Royalties**: Developers earn 30% of the gas paid for smart contract execution.
- **WebAssembly (WASM) VM**: Support for smart contracts written in any language that compiles to WASM.

### **Rust Framework**
- Low-level, multi-paradigm language optimized for performance and safety.
- Enables high-efficiency, low-gas smart contracts.

---

## Getting Started


## Installation

### **1. Install `pipx`**
`pipx` is the recommended way to install the MultiversX CLI (`mxpy`).

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

Confirm installation:
```bash
pipx --version
```

---

### **2. Install `mxpy`**
The MultiversX CLI (`mxpy`) is a versatile tool for interacting with the blockchain and managing smart contracts.

Install `mxpy`:
```bash
pipx install multiversx-sdk-cli --force
```

---

### **3. Install Rust**
Rust is essential for compiling smart contracts.

#### Ubuntu & Windows WSL
```bash
sudo apt install build-essential pkg-config libssl-dev
```

#### macOS
```bash
xcode-select --install
```

Install Rust using `mxpy`:
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

To create an empty smart contract project:
```bash
sc-meta new --template empty --name simple-voting
```

Verify the project setup:
```bash
cargo check
```

---

## Building the Smart Contract

Compile the smart contract source code into WebAssembly (WASM) bytecode:
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
- **`simple-voting.wasm`**: The WASM bytecode to deploy to the blockchain.
- **`simple-voting.abi.json`**: The Application Binary Interface (ABI) defines how to interact with the smart contract.

---

## Deploying the Smart Contract in devnet

To deploy the compiled contract:
```bash
mxpy --verbose contract deploy \
  --recall-nonce \
  --bytecode="./output/simple-voting.wasm" \
  --keyfile="../<your-wallet-keyfile>.json" \
  --gas-limit=100000000 \
  --proxy="https://devnet-gateway.multiversx.com" \
  --chain="D" \
  --send
```

Replace `<your-wallet-keyfile>.json` with your wallet file name. The command will ask for your password twice.

Take note of the contract address in the terminal.

---

