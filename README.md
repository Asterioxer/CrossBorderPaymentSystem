# Blockchain Based Cross-Border Payment System

A private, permissioned blockchain application built on **Hyperledger Fabric** for tracking, validating, and processing cross-border banking transactions. The network models real inter-bank settlement — including multi-currency exchange between banks (e.g. a USD bank and an Indian bank) — with smart contracts enforcing transaction integrity and an embedded explorer for transparent monitoring of the ledger.

This repository is a **debugged and hardened fork** of the original CrossBorderPaymentSystem project. The base implementation had several critical, application-breaking issues that made the system non-functional end-to-end; this fork resolves them to deliver a working, demoable banking platform.

## What Was Fixed

The original codebase did not work out of the box. The following core flows were broken and have been repaired in this fork:

- **Bank login & signup** — Bank registration and authentication were failing due to broken session/identity handling. Fixed the registration and login pipeline so banks can be onboarded and authenticated correctly against the Fabric network.
- **Customer login & signup** — Customer-side registration and authentication had the same class of failures, preventing customers from ever reaching the dashboard. Fixed identity enrollment and session handling so customer accounts work end-to-end.
- **Inter-customer transactions** — Transfers between two customers were failing outright. Debugged and repaired the transaction submission flow so customer-to-customer transfers complete and settle correctly on-chain.
- **Cross-border currency exchange** — Transactions between banks in different currencies (e.g. USD ↔ INR) were erroring out instead of converting and settling. Fixed the exchange/conversion logic in the chaincode and application layer so cross-currency, cross-bank transactions now process correctly and reflect accurate converted amounts on both ends of the transfer.

The result is a fully usable, runnable demo: banks and customers can register, log in, and move money — including across currencies and across banks — with the ledger correctly reflecting every transaction via the Hyperledger Explorer.

## Project Description

This project is a blockchain application developed on the Hyperledger Fabric framework. It includes smart contracts (chaincode) used for tracking, validating, and processing cross-border payments. A permissioned network has been established for transactions between different banks, with the Hyperledger Explorer providing transparent, real-time monitoring of payment processes on the ledger.

## Installation

To run this project in a local development environment, follow the steps below:

**Requirements:**
- Docker
- Node.js

### Terminal Commands

### Starting the Network and Running the Application

1. Navigate to the `cbps-network` directory:

```bash
cd cbps-network
```

2. Run the following command to start the network and create a channel:

```bash
./network.sh up createChannel -c bankschannel -ca -s couchdb0
```

3. Deploy the chaincode by running the following command:

```bash
./network.sh deployCC -c bankschannel -ccn bank -ccp ../crossBorderPayment/chaincode-go/ -ccl go -ccep "OR('Org1MSP.peer','Org2MSP.peer')"
```

4. Navigate to the `crossBorderPayment/application` directory:

```bash
cd ../crossBorderPayment/application
```

5. Install the required dependencies using npm:

```bash
npm install
```

6. Run the following commands to enroll the admin users:

```bash
node enrollAdmin.js org1
node enrollAdmin.js org2
```

7. Start the application by running the following command:

```bash
node app.js
```

If there are no errors, the application will be accessible at `http://localhost:3000`, where bank and customer login/signup are fully functional, and transactions — including cross-currency, cross-bank transfers — can be submitted successfully.

![Bank and Customer LogIn/SignUp Page](images/mainpage.png)

### Explorer Configuration

1. Modify the necessary files for the explorer:

   - Take the name of the file in the `cbps-network/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/keystore` directory.
   - Change the end of the `adminPrivateKey` value in the `explorer/cbps-network.json` file.

2. Navigate to the `explorer` directory:

```bash
cd explorer
```

3. Run the following command to start the Docker containers:

```bash
docker-compose up
```

![Explorer](images/Exploerer_mainpage.png)

4. If there are no errors, the explorer interface will be accessible at `http://localhost:8080`, where you can verify that all transactions — including the fixed cross-bank currency exchanges — are correctly recorded on the ledger.

### Stop and Clear Network

1. Navigate to the `cbps-network` directory:

```bash
cd cbps-network
```

2. Run the following command to shut down the network:

```bash
./network.sh down
```

3. Navigate to the `crossBorderPayment/application` directory:

```bash
cd ../crossBorderPayment/application
```

4. Delete the `wallet` and `node-modules` directories:

```bash
rm -rf wallet
rm -rf node-modules
```

Stopping and Removing Docker Containers:

```bash
docker stop $(docker ps -a -q)
docker rm -f $(docker ps -aq)
docker system prune -a
docker volume prune
```

Listing Docker Containers and Images:

```bash
docker ps -a
docker images -a
docker volume ls
```

## Credits

Forked and debugged from the original [CrossBorderPaymentSystem](https://github.com/Mervespm/CrossBorderPaymentSystem) project. Core authentication, transaction, and currency-exchange flows were audited and repaired to bring the application to a working state. See [CHANGELOG.md](CHANGELOG.md) for the full breakdown of fixes.
