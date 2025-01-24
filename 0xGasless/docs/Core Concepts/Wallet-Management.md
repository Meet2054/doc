---
sidebar_position: 4
---
## Wallet Management

AgentKit uses the CDP SDK to manage wallets and perform onchain operations. The CDP SDK supports a wide variety of actions, including:

- Creating MPC wallets
- Signing transactions
- Deploying and interacting with tokens
- Invoking smart contracts and querying chain state

There are two options for giving an agent access to a wallet:

1. **Provide a mnemonic phrase in the `.env` file.**

```bash
export MNEMONIC_PHRASE="your_mnemonic_phrase" # Optional
```

2. **Let the agent create a new wallet.** If a mnemonic phrase is not provided, the agent will create a new 1-of-1 developer wallet.

### By default, AgentKit supports the following tools:

- `deploy_nft` - Deploy new NFT contracts
- `deploy_token` - Deploy ERC-20 token contracts
- `get_balance` - Get balance for specific assets
- `get_balance_nft` - Get balance for specific NFTs (ERC-721)
- `get_wallet_details` - Get details about the MPC Wallet
- `mint_nft` - Mint NFTs from existing contracts
- `morpho_deposit` - Deposit into a Morpho Vault
- `morpho_withdraw` - Withdraw from a Morpho Vault
- `pyth_fetch_price` - Fetch the price of a given price feed from Pyth Network
- `pyth_fetch_price_feed_id` - Fetch the price feed ID for a given token symbol from Pyth Network
- `register_basename` - Register a Basename for the wallet
- `request_faucet_funds` - Request test tokens from the Base Sepolia faucet
- `trade` - Trade assets (mainnets only)
- `transfer` - Transfer assets between addresses
- `transfer_nft` - Transfer an NFT (ERC-721)
- `wow_buy_token` - Buy Zora Wow ERC-20 memecoin with ETH (Base only)
- `wow_create_token` - Deploy a token using Zora's Wow Launcher (Bonding Curve) (Base only)
- `wow_sell_token` - Sell Zora Wow ERC-20 memecoin for ETH (Base only)
- `wrap_eth` - Wrap ETH to WETH

Any action not supported by default by AgentKit can be added by adding agent capabilities.

### Supported Networks

AgentKit supports every network that the CDP SDK supports.

To switch networks, you can update the `NETWORK_ID` environment variable in the `.env` file, or by executing the following command:

```bash
export NETWORK_ID=<NETWORK_ID> # e.g. "base-sepolia", "ethereum-mainnet", "arbitrum-mainnet", etc.
