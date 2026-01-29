# FundMe

A decentralized crowdfunding smart contract built with Solidity and Foundry. Users can contribute ETH with automatic USD conversion via Chainlink price feeds, and the contract owner can withdraw accumulated funds.

## Features

- ETH contributions with minimum USD threshold ($5)
- Real-time ETH/USD price conversion using Chainlink oracles
- Owner-only withdrawal functionality
- Gas-optimized withdraw function
- Multi-network deployment support (Sepolia, Mainnet, Anvil)
- Comprehensive test suite with unit and integration tests

## Prerequisites

- [Foundry](https://book.getfoundry.sh/getting-started/installation)
- Git

## Installation

```bash
git clone https://github.com/GN25/foundry-fundMe.git
cd foundry-fundMe
forge install
```

## Usage

### Build

```bash
forge build
```

### Test

```bash
# Run all tests
forge test

# Run with verbosity
forge test -vvv

# Run specific test
forge test --match-test testFundFailsWithoutEnoughEth
```

### Coverage

```bash
forge coverage
```

### Deploy

**Local Anvil:**
```bash
# Start local node
anvil

# Deploy (in another terminal)
forge script script/DeployFundMe.s.sol --rpc-url http://localhost:8545 --private-key <PRIVATE_KEY> --broadcast
```

**Sepolia Testnet:**
```bash
forge script script/DeployFundMe.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast --verify
```

### Interact

**Fund the contract:**
```bash
forge script script/Interactions.s.sol:FundFundMe --rpc-url <RPC_URL> --private-key <PRIVATE_KEY> --broadcast
```

**Withdraw funds:**
```bash
forge script script/Interactions.s.sol:WithdrawFundMe --rpc-url <RPC_URL> --private-key <PRIVATE_KEY> --broadcast
```

## Contract Architecture

- **FundMe.sol**: Main contract handling contributions and withdrawals
- **PriceConverter.sol**: Library for ETH/USD price conversions
- **HelperConfig.s.sol**: Network-specific configuration management
- **MockV3Aggregator.sol**: Mock Chainlink oracle for testing

## Security

- Uses custom errors for gas efficiency (`FundMe__NotOwner`)
- Implements access control with `onlyOwner` modifier
- Tested withdrawal patterns using `call` method

## Gas Optimization

The contract includes a `cheaperWithdraw` function that caches array length to reduce SLOAD operations, saving gas on withdrawals with multiple funders.

## License

MIT
