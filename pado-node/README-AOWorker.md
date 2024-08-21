
- [AO Worker](#ao-worker)
  - [Install](#install)
  - [Basic Configurations](#basic-configurations)
    - [Node Info](#node-info)
    - [Arweave Wallet](#arweave-wallet)
    - [LHE Key](#lhe-key)
  - [Register to PADO AO Process](#register-to-pado-ao-process)
  - [Run Task](#run-task)
  - [Add New Workers](#add-new-workers)
    - [Add EigenLayer Worker](#add-eigenlayer-worker)


# AO Worker


## Install

Please reference to official documentation: [Install Docker Engine](https://docs.docker.com/engine/install/).

Next, pull the image:

```shell
docker pull padolabs/pado-network:latest
```

Clone [pado-labs/pado-worker-setup](https://github.com/pado-labs/pado-worker-setup):
```sh
git clone https://github.com/pado-labs/pado-worker-setup.git
cd pado-worker-setup/pado-node
```

## Basic Configurations

Copy `./config-files/.env.ao` into `./.env`. Edit the `./.env` and update the values for your own setups.


### Node Info

Set a name to identify yourself, these will be used on the node itself and will be shown on performance metrics in the future.

```sh
NODE_NAME="Your Node Name"
NODE_DESCRIPTION="Your Node Name's Description"
```


### Arweave Wallet

If you don't have an Arweave wallet, you can install one from [ArConnect](https://www.arconnect.io/download), and then export the wallet from ArConnect and store it to somewhere.

Next, fill in the file path of the Arweave wallet,

```sh
AR_WALLET_PATH='/path/to/your/arwallet.json'
```


### LHE Key

The LHE key is used for data sharing, use the following command to generate it.

```sh
bash ./run.sh generate-lhe-key [--key-name <NAME>]
```

The default output is `./keys/default.lhe.key.json`, you can specify the key name via `--key-name <NAME>`.

**IMPORTANT!** Don't lose this file and save it to a safe place!

Next, fill in the file path of the LHE key you have generated.

```sh
LHE_KEY_PATH='/path/to/your/lhe.key.json'
```




## Register to PADO AO Process

**NOTE:** *Please contact [PADO Labs](https://discord.gg/YxJftNRxhh) to add your wallet address to the WHITELIST before being able to successfully register!*

Once the configuration is complete, you can run:

```sh
bash ./run.sh ao:register
```

<br/>

In general, you only need to perform the registry step once.


## Run Task

Once successfully registered, you can start the task program.

```sh
bash ./run.sh task [<name>]
```

It will start a container named `pado-network[-name]` in the background. Some logs will output to `./logs/*.log`.

You can Stop/Start/Restart/Remove the container by running `docker stop/start/restart/rm pado-network[-name]`.


## Add New Workers

### Add EigenLayer Worker

Step 1: Reference `./config-files/.env.holesky`(Holesky), mainly copy and append the following options and their value to `.env`:

```sh
ENABLE_EIGEN_LAYER
ETH_RPC_URL
REGISTRY_COORDINATOR_ADDRESS
ROUTER_ADDRESS
ECDSA_KEY_FILE
ECDSA_KEY_PASSWORD
BLS_KEY_FILE
BLS_KEY_PASSWORD
```

Step 2: Set your own `ECDSA_KEY_FILE`, `ECDSA_KEY_PASSWORD`, `BLS_KEY_FILE`, `BLS_KEY_PASSWORD`. Reference [Register as Operator on EigenLayer](./README-EigenLayerWorker.md#register-as-operator-on-eigenlayer) and [ECDSA and BLS Key](./README-EigenLayerWorker.md#ecdsa-and-bls-key).

Step 3: Deposit some ETH to everPay. Reference [Storage](./README-EigenLayerWorker.md#storage).

Step 4: Register to PADO AO Process. Reference [Register to PADO AVS](./README-EigenLayerWorker.md#register-to-pado-avs).

Step 5: Remove the old container and re-run the task. Reference [Run Task](#run-task).

<br/>

You can see the full configuration options from `./config-files/.env.holesky-and-ao`.

