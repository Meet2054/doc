---
sidebar_position: 1
---

# Agent Actions

### By default, AgentKit supports the following tools:

- `deploy_nft` - Deploy new NFT contracts
- `deploy_token` - Deploy **ERC-20** token contracts
- `get_balance` - Get balance for specific assets
- `get_balance_nft` - Get balance for specific NFTs (ERC-721)
- `get_wallet_details` - Get details about the MPC Wallet
- `mint_nft` - Mint NFTs from existing contracts
- `morpho_deposit` - Deposit into a [Morpho Vault](https://morpho.org)
- `morpho_withdraw` - Withdraw from a  [Morpho Vault](https://morpho.org)
- `pyth_fetch_price` - Fetch the price of a given price feed from [Pyth Network](https://www.pyth.network)
- `pyth_fetch_price_feed_id` - Fetch the price feed ID for a given token symbol from  [Pyth Network](https://www.pyth.network)
- `register_basename` - Register a **0xGaslessname** for the wallet
- `request_faucet_funds` - Request test tokens from the **0xGasless faucet**
- `trade` - Trade assets (mainnets only)
- `transfer` - Transfer assets between addresses
- `transfer_nft` - Transfer an NFT (ERC-721)
- `wow_buy_token` - Buy **Zora Wow** ERC-20 memecoin with ETH (Base only)
- `wow_create_token` - Deploy a token using **Zora's Wow Launcher** (Bonding Curve) (Base only)
- `wow_sell_token` - Sell **Zora Wow** ERC-20 memecoin for ETH (Base only)
- `wrap_eth` - Wrap ETH to WETH

Any action not supported by default by AgentKit can be added by adding agent capabilities.
