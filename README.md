# cargo-l1x-contract

Using CLI
```
l1x-cli-beta contract deploy CONTRACT_OBJECT_FILE_PATH [--endpoint] [--from] [--fee_limit] [--nonce]
l1x-cli-beta contract init CONTRACT_ADDRESS --args [--endpoint] [--from] [--fee_limit] [--nonce]
l1x-cli-beta contract view CONTRACT_ADDRESS FUNCTION --args [--endpoint] [--from]
l1x-cli-beta contract call INIT_CONTRACT_ADDRESS FUNCTION --args [--endpoint] [--from] [--fee_limit] [--nonce]
```

Using Hardhat
- contract file
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract Token is ERC20 {
    constructor(uint256 initialSupply) ERC20("MyToken", "MTK") {
        _mint(msg.sender, initialSupply);
    }
}
```
- deploy.js
```
// scripts/deploy_myerc20.js
const { ethers } = require("hardhat");

async function main() {
  const [deployer] = await ethers.getSigners();

  console.log("Deploying contracts with the account:", deployer.address);

  const Token = await ethers.getContractFactory("Token");
  const initialSupply = "1000000000000000000000"; // 1000 tokens
  const name = "L1XToken";
  const symbol = "XT";
  const decimals = 18;

  const token = await Token.deploy(
    initialSupply,
    // name,
    // symbol,
    // decimals
  );

  // await token.waitForDeployment();

  console.log("Token deployed to:",await token.getAddress());
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

- harthat config file
```
import { HardhatUserConfig } from "hardhat/config";
import "@nomicfoundation/hardhat-toolbox";
import '@nomicfoundation/hardhat-ethers';

// Enter your Private Key here
const PRIVATE_KEY = 'xx'; 

const config: HardhatUserConfig = {
  solidity: {
    version: "0.8.20",
    settings: {
      optimizer: {
        enabled: true,
      },
    },
  },
  networks: {
    localhost: {
      url: 'localhost:8545',
    },
    l1xTestnet: {
      url: '<https://v2-testnet-rpc.l1x.foundation>',
      accounts: [PRIVATE_KEY], 
    },
  },
};

export default config;
```
- excute command
```
npx hardhat run scripts/deploy.ts --network l1xTestnet
```
