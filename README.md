# Blockchain Programming Cheat sheet

## 1. Metamask

This can be used to hold an account inside Chrome for easier access to test wallet accounts - Use Rinkeby for testing

##### 1.1 Funding account

1. Create social post with address
2. Copy the post's link inside https://faucet.rinkeby.io/ to fund your test account

## 2. Language

The programming language to be used is called <b>Solidity</b>. The documentation can be found at https://docs.soliditylang.org/en/v0.8.0/.

## 3. Local Setup

1. Install NodeJS 14.15.0
2. Create folder
3. Go into folder
4. npm init (creates package.json)
5. npm install -g truffle
6. truffle init

#### 3.1 To create a smart contract

truffle create contract %contractName%

#### 3.2 To compile

truffle compile

#### 3.3 Develop

You can also run <b>truffle develop</b> and then run commands such as compile and migrate inside it. 

#### 3.4 Migrate

Running <b>migrate</b> in develop migrates contracts to the blockchain.

##### 3.4.1 To create a migration

truffle create migration %migrationName%.

Then you can add the following code to newly added file in the migrations directory.

```javascript
const ContractName = artifacts.require('ContractName')

module.exports = function(deployer){
    deployer.deploy(ContractName)
}
```

##### 3.4.2 If already migrated

Add <b>--reset</b> to restart the migration


#### 3.5 Other notes

##### 3.5.1 Issues on windows

Check out https://www.trufflesuite.com/docs/truffle/reference/configuration#resolvingnaming-conflicts-on-windows

##### 3.5.2 Changing solc version

After running <b>truffle init</b>, one should find a file named <b>truffle-config.json</b>. Change the version under compilers > solc to <b>0.6.6</b>.

```javascript
compilers: {
    solc: {
      version: "0.6.6"
    },
  }
```

## 4. Testing

#### 4.1 Mocha

<b>truffle develop</b> uses Mocha for tests, whose documentation can be found at https://mochajs.org/.

#### 4.2 Chai

<b>truffle develop</b> uses Chai for assertions, whose documentation can be found at https://www.chaijs.com/api/assert/ .

## 5. Deploying Smart Contract To Rinkeby Network

To deploy a smart contract once can use a public node.

#### 5.1 Infura

1. Go to infura.io
2. Sign Up
3. Create new project
4. Get the endpoint from the project's settings under KEYS. <b>Make sure Rinkeby is selected in the selectbox near ENDPOINTS</b>

#### 5.2 Setup

1. Go into the directory of the newly created project(locally not on Infura)
2. Run <b>npm install @truffle/hdwallet-provider</b> or <b>npm install truffle-hdwallet-provider </b> for older versions.
3. Open <b>truffle-config.js</b>
4. Get the mnemonic/seed from the MetaMask setting (Click top-right circle > settings > Security & Privacy > Reveal Seed Phrase). This should be like a long phrase with different words making no sense. 
5. Make sure the truffle-config.js file looks like this:
```javascript
const metaMaskMnemonic = "seed obtained from metamask's settings";

const HDWalletProvider = require('@truffle/hdwallet-provider');

module.exports = {
  networks: {
    rinkeby: {
      network_id: 4,
      provider: new HDWalletProvider(metaMaskMnemonic, "endpoint obtained from infura's settings")
    }
  },

  // Set default mocha options here, use special reporters etc.
  mocha: {
    // timeout: 100000
  },

  // Configure your compilers
  compilers: {
    solc: {
      version: "0.6.6"
    },
  },
};
```
6. Run <b>truffle migrate --network rinkeby</b>


#### 5.3 Checking Deployment

The migrate command should return a contract address and this can be checked by going into  https://rinkeby.etherscan.io/ and pasting the address there. 