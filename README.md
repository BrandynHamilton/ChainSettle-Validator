# ChainSettle Validator Node

ChainSettle Validator Nodes listen to on-chain `Attested` events and vote on settlements using a registered validator wallet. These nodes are essential to the decentralized confirmation of verified off-chain actions (like GitHub milestones, fiat payments, or document signings).

---

## Requirements

- Python 3.8+
- Web3-compatible wallet (can be generated automatically)
- Internet access (RPC endpoints and backend API)
- API key (if registering a new validator)

---

## Getting Started

### 1. Clone this Repository

```bash
git clone https://github.com/YOUR_USERNAME/chainsettle-validator
cd chainsettle-validator
```

### 2. Set Up Environment

Copy the sample file and modify it:

```bash
cp .env.sample .env
```

The following environment variables are required:

```env
ALCHEMY_API_KEY=your_key_here
FAUCET_URL=http://localhost:5000/faucet
LOCAL_URL=http://localhost:5000
VALIDATOR_API_KEY=your_registration_key
VALIDATOR_NODE_KEY=optional_private_key
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## Commands

### `listen`

This is the primary command to run a validator node:

```bash
python validator.py listen --network ethereum
```

```bash
python validator.py listen --network blockdag
```

You can also run across all networks:

```bash
python validator.py listen --network all
```

Optional flags:
- `--new-wallet`: generate and encrypt a new keystore if none exists
- `--account <address>`: use or unlock a specific keystore by address

---

## Behavior Overview

1. On startup, the validator:
   - Loads a keystore from the environment or disk.
   - If none is found, it prompts to generate one (or uses `--new-wallet`).
   - Connects to the specified network(s).
   - Checks if the address is a registered validator.
   - If not, attempts registration using the provided `VALIDATOR_API_KEY`.

2. If balance is below 0.01 ETH:
   - Automatically attempts to claim funds from the faucet.

3. Once setup is complete:
   - Listens to `Attested` events on-chain.
   - Submits a transaction to `voteOnSettlement()` with onchain confirmation.

---

## Keystore Directory

Encrypted keystores are stored in:

```
keystores/
```

This folder is `.gitignore`d by default.

---

## Example Setup

```bash
python validator.py listen --network all --new-wallet
```

---

## Deployment Notes

- Supports multi-network validation (e.g., Sepolia and BlockDAG)
- Automatically retries failed RPC connections
- Compatible with ChainSettle smart contracts deployed to Ethereum and other EVM chains