---
id: dapploy
title: DAPPLOY
sidebar_label: Dapploy
---

> The XYO Foundation provides this source code available in our efforts to advance the understanding of the XYO Procotol and its possible uses. We continue to maintain this software in the interest of developer education. Usage of this source code is not intended for production. 


We've gone through the hoops of deploying dApps on the Ethereum Network and have nuked out all of the annoying parts to make dapployment a breeze.

<div class="alert alert-info text-center" role="alert">
  Difficulty Level: Intermediate
</div>

## Getting Started

1. Install from source to get access to the sample projects. Remember, 'YARN ALL THE THINGS' from the main project dir by running this:

```
yarn && cd samples/sample-datavault-react/ && yarn && cd ../sample-datavault-solidity && yarn && cd ../..
```

2. _OR_ globally install Dapploy using:

```
sudo yarn global add tool-dapploy-nodejs
OR
sudo npm install -g tool-dapploy-nodejs --unsafe-perm=true --allow-root
```

3. Run `dapploy -h` to see the options:

```
 $> dapploy -h
Usage: dapploy [options] [command]

Options:

  -V, --version                                 output the version number
  -t, --projectDir <dir>                        Truffle Project Directory
  -n, --network [network]                       Deploy to network (default: development)
  -c, --config <config>                         Config file (default: .dapploy)
  -o, --contractOutput <dir>                    Contract Output Directory
  -x, --excludeContracts [Contract1,Contract2]  Exclude contracts from the web3 interface (files are still copied)
  -a, --includeContracts [Contract1,Contract2]  Include contracts from the web3 interface (files are still copied)
  -l, --clean                                   Clean contracts before migrating
  -r, --remoteOnly                              Only copy contracts remote
  -p, --pinToIpfs                               Pin contracts to IPFS for remote access
  -k, --bucketName                              Do remote copy of ABI to aws bucket (make sure to setup ~/.aws)
  -y, --copyOnly                                Only do folder copy and S3 copy
  -i, --init                                    Add default config file to current directory
  -h, --help                                    output usage information

  $> dapploy init -h
Usage: init [options] [dir]

Configure a new truffle project from scratch

Options:

  -g, --configOnly                  Create dapploy config file in current directory
  -s, --specifyContract [contract]  Which type of project to create. Supporting: [ERC20, ERC721] (default: ERC20)
  -h, --help                        output usage information
```

4. If you did not add dapploy globally with '-g', Add dapploy alias to run from any project in your bash_profile:

```
echo "alias dapploy=\"/<path_to_project>/tool-dapploy-nodejs/dapploy\""| cat >> .bash_profile
```

## Create a smart contract project

### `dapploy init` - create a new smart contract project

1. Create a new ERC20 project using dapploy:

```
dapploy init my-first-coin -s ERC20
cd my-first-coin
```

2. Make sure Ganache is downloaded, installed and open: [Download and install Ganache from their site](https://truffleframework.com/ganache)

- Click the Gear Icon thingy ( ⚙️ ) to open `Preferences...`
  Make sure that port is set to 8545.

3. Dapploy from root directory

- if you have alias setup for dapploy, just run

```
  dapploy
```

- otherwise run

```
  </path/to>/dapploy
```

Congrats, you just built your first ERC-20 Token! Head on over to [Dapper](https://github.com/XYOracleNetwork/tool-dapper-react) to play with it in a web-ui

4. Checkout some dapploy samples to get another Dapp built with dapploy that uses IPFS and Dapploy's web3 adapter.

```
cd samples/sample-datavault-solidity && dapploy

cd ../sample-datavault-react && yarn start
```

## Making your ABI public

### dApploy to IPFS

```
dapploy -p

 $ Contracts stored to IPFS QmZ2Ezv4nsQ5tqGoHz4pUxa9GW88TWKMSzdxdMfxsHerWT
 $ View contracts at https://ipfs.xyo.network/ipfs/QmZ2Ezv4nsQ5tqGoHz4pUxa9GW88TWKMSzdxdMfxsHerWT
```

## Dapploy to public testnet and mainnet

1. [Sign up for Infura](https://infura.io/)

2. [Install MetaMask from their site](https://metamask.io/)

- Sign into Metamask and change Network on Metamask to kovan/ropsten/mainnet

3.  Add your metamask wallet and infura data to `env.template` file:

```
WALLET=// wallet used to deploy contracts
INFURA_API_KEY=// API key you get from infura
MNENOMIC=// 12 word pass key from derived from your wallet private key
```

4.  Move `env.template` to `.env` (this file is already in your .gitignore)

```
mv env.template .env
```

5.  Using kovan, run

```
dapploy -n kovan
```

Ropsten:

```
dapploy -n ropsten
```

_NOTE_ You don't need to specify `-n network` if you change `.dapploy` configuration file in your project from `network=development` to `network=kovan` etc.

```
vi .dapploy
```

If you are feeling adventurous run a local testnet node:

### [Ropsten testnet node setup (Geth)](https://github.com/XYOracleNetwork/tool-dapploy-nodejs/wiki/Local-Ropsten-Config)

### [Kovan testnet node setup (Parity)](<https://github.com/XYOracleNetwork/tool-dapploy-nodejs/wiki/Kovan-setup-(Parity)>)

## Multiple Browser Support 

### Portis

1.  Setup account with [portis.io](https://portis.io)

2.  Add Portis and Infura key to .dapploy

```
vi .dapploy
```

Uncomment:

```
# [Portis]
# portisApiKey=<API_KEY>
# infuraApiKey=<INFURA_API_KEY>
```

### Amazon S3

1.  Confugure your AWS credentials in terminal by creating credentials file. [S3 credential instructions here](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html):

```
  vi ~/.aws/credentials
  [default]
  aws_access_key_id = AKIAIFYQ4UEUUJ6GDH6A
  aws_secret_access_key = FAKEasdfas=aqewrasdfa/sdfasdfasdfasdfFAKE
```

2. Configure dapploy

```
vi .dapploy
```

Uncomment:

```
#[AWS]
#bucketName=layerone.smart-contracts
#remotePath=ABI
```
