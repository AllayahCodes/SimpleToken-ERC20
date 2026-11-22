# SimpleToken (ERC20)

A minimal ERC20-style token contract written in Solidity. Contract only, no frontend.

## Overview

`SimpleToken` implements the core ERC20 functionality: a fixed supply minted to the deployer, direct transfers, and approved third-party spending.

## Contract Details

- **File:** `SimpleToken.sol`
- **License:** MIT
- **Solidity:** `^0.8.25`

### State Variables

| Variable | Description |
|---|---|
| `name` | Full token name |
| `symbol` | Trading symbol |
| `decimals` | Token divisibility (typically 18) |
| `totalSupply` | Total tokens in circulation (`_initialSupply * 10**decimals`) |
| `balanceOf` | Mapping of address to balance |
| `allowance` | Mapping of owner to spender to approved amount |

### Events

- `Transfer(from, to, value)`: emitted on transfers and on the initial mint (from `address(0)`)
- `Approval(owner, spender, value)`: emitted when an allowance is set

### Functions

- `transfer(_to, _value)`: send tokens to another address
- `approve(_spender, _value)`: authorise a spender
- `transferFrom(_from, _to, _value)`: spend tokens from an approved allowance

### Constructor

```
constructor(string _name, string _symbol, uint8 _decimals, uint256 _initialSupply)
```

The initial supply is multiplied by `10**decimals` inside the constructor, so pass whole tokens (e.g. `1000`), not the raw unit amount. The full supply is assigned to the deployer.

## Deploying in Remix

1. Open [remix.ethereum.org](https://remix.ethereum.org) and create `SimpleToken.sol`.
2. In the Solidity Compiler tab, select version `0.8.25` or higher and compile.
3. In Deploy & Run Transactions, set the environment to **Remix VM**.
4. Enter the constructor arguments:
   - `_name`: `My Student Token`
   - `_symbol`: `MST`
   - `_decimals`: `18`
   - `_initialSupply`: `1000`
5. Click **Deploy**.

## Testing

Tested manually in Remix VM with two accounts:

1. **Metadata:** `name`, `symbol`, `decimals` and `totalSupply` return the deployed values.
2. **Mint:** the deployer's `balanceOf` equals `totalSupply`.
3. **Transfer:** send 1 token (`1000000000000000000`) from Account 1 to Account 2 and confirm both balances.
4. **Approve:** Account 1 approves Account 2, then `allowance` returns the approved amount.
5. **TransferFrom:** Account 2 calls `transferFrom` for Account 1, then `allowance` drops by the amount moved and the recipient's balance increases.
6. **Reverts:** `transfer` above the balance fails with `Insufficient balance`, and `transferFrom` above the allowance fails with `Insufficient allowance`.

## Notes

- Values are in the token's smallest unit. With 18 decimals, 1 token is `1000000000000000000`.
- SafeMath is not used. Solidity 0.8+ reverts on arithmetic overflow and underflow by default.
