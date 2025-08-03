# Foundry Fundamentals – Lesson 2: Fund Me

> A minimal, opinionated Foundry project that implements and tests a **crowd-funding smart contract** powered by Chainlink price feeds.

This repository follows along with [Patrick Collins’ “Foundry Fundamentals” course](https://github.com/smartcontractkit/foundry-fund-me).  Lesson 2 covers writing a simple `FundMe` contract, wiring up deployment & interaction scripts, and building a healthy test-suite with Foundry.

## ✨ What you’ll find inside

- **`src/`** – Production contracts
  - `FundMe.sol` – Main contract that lets anyone deposit ETH as long as the value ≥ $5 (USD).  Only the owner can withdraw.
  - `PriceConverter.sol` – A small library that converts an ETH amount to USD via `AggregatorV3Interface`.
- **`script/`** – On-chain automation
  - `DeployFundMe.s.sol` – Deploys a fresh `FundMe` pointing at the appropriate Chainlink price feed for the active network.
  - `HelperConfig.s.sol` – Resolves the correct price feed address for Anvil, Sepolia or Mainnet (deploys a mock on local networks).
  - `Interactions.s.sol` – Example scripts to fund and withdraw from the latest deployment.
- **`test/`** – Foundry test-suites
  - Unit tests (`test/unit/*`) that exercise contract logic in isolation.
  - Integration tests (`test/integration/*`) that execute the interaction scripts against a fork.
- **CI** – GitHub Actions workflow (`.github/workflows/test.yml`) that fmt / builds / tests every push.

## 🏗  Prerequisites

1. **Foundry** – Install with
   ```bash
   curl -L https://foundry.paradigm.xyz | bash
   foundryup
   ```
2. **(Optional) Environment variables** – To deploy to Sepolia / Mainnet you’ll need:
   ```bash
   # .env
   SEPOLIA_RPC_URL=<your_alchemy_or_infura_endpoint>
   ETHERSCAN_API_KEY=<api_key_for_verification>
   ```
   The included `Makefile` expects these when broadcasting.

## 🚀 Quick start

Clone the repo and install dependencies (submodules are already vendored in `lib/`).

```bash
# 1️⃣  Build & test everything locally
forge build          # compile
forge test -vvv      # run tests with verbose traces

# 2️⃣  Spin up an Anvil node in another terminal (optional)
anvil

# 3️⃣  Deploy to the local chain
forge script script/DeployFundMe.s.sol:DeployFundMe \
  --fork-url http://127.0.0.1:8545 \
  --broadcast -vvvv
```

### Deploy to Sepolia

```bash
make deploy-sepolia   # wraps the forge script command found in the Makefile
```

After a successful broadcast, the deployment artifacts are stored in `broadcast/`.

### Interact with the latest deployment

```bash
# fund with 0.01 ETH
forge script script/Interactions.s.sol:FundFundMe \
  --broadcast -vvvv --rpc-url $SEPOLIA_RPC_URL

# withdraw funds (owner only)
forge script script/Interactions.s.sol:WithdrawFundMe \
  --broadcast -vvvv --rpc-url $SEPOLIA_RPC_URL
```

## 📂 Directory layout

```
├── src/                  # Production contracts
├── script/               # Deployment & interaction scripts
├── test/                 # Unit and integration tests
├── broadcast/            # Forge deployment artifacts (generated)
├── lib/                  # External dependencies (via forge install)
├── foundry.toml          # Foundry configuration
└── Makefile              # Helper targets
```

## 🤝 Contributing

Feel free to open issues or PRs if you find a bug or have an improvement.  This repo is mostly educational, so suggestions that make the examples clearer are very welcome.

## 📝 License

MIT © 2023 Patrick Collins – Modifications by Adrian

