Installation guide
-----------------

### Prerequisites

1 - Docker and docker compose. Installation instructions:  
https://docs.docker.com/install/linux/docker-ce/ubuntu/  
https://docs.docker.com/compose/install/  
2 - Sentry instance – optional but highly recommended. Installation instructions:
https://hub.docker.com/_/sentry/  
3 - SMTP server - you can either use an existing provider, or run your own.  
4 - Bitcoind node and/or Litecoind node. The configuration is currently set to LTC, but CryptoInvoice supports both currencies.  

### Installation steps

1 - Clone this repository using SSH (submodules don't work with HTTPS)  
2 - Run `git submodule init` and `git submodule update`  
3 - Set connection to Sentry in `webapi/WebApi/appsettings.json` and `blockchain-observer/BlockchainObserver/appsettings.json`  
4 - Start a litecoind node: https://github.com/litecoin-project/litecoin  
5 - Set the credentials for the JSON RPC API of your Litecoin node in `blockchain-observer/BlockchainObserver/appsettings.json`.  
6 - Set your IP and port where WebAPI will run in `frontend/src/appSettings.json`  
7 – Go the root folder of this project, where you see the docker-compose.yml file and run `sudo docker-compose up -d --build`. This will create all containers, including MariaDB database and RabbitMQ message broker.
8 - Add mail queue endpoint to scheduler: open `crontab -e` and add `*/2 * * * * curl http://127.0.0.1:8080/api/Email/sendFromQueue` - this will send new email messages from queue every 2 minutes. Replace "127.0.0.1:8080" with IP and port of your WebAPI.  
9 – Open the UI and register a user account.  

### For UI development

1 - Install Node.js and NPM - https://nodejs.org  
2 - Go to the frontend repository of CryptoInvoice  
3 - Set your IP and port in `frontend/src/appSettings.json` to a running WebAPI instance (change the development settings, not production)  
4 - Run `npm install` - this will install all frontend dependencies.  
5 - Run `npm run dev` - this will start the webpack dev server for frontend on your localhost.  

### For extending WebAPI or Blockchain Observer

Both are written in C# / .NET Core 2.0, which is cross platform. You can use Visual Studio Code to develop .NET Core apps on Linux, Mac and Windows - https://code.visualstudio.com/

### Other

If you would like to develop an alternative blockchain observer for another currency, all you need to do is connect your service to RabbitMQ and communicate using JSON RPC notifications. A specification of the API is in the **wiki** for this repository.

The wiki also contains documentation for the REST API used by frontend. It has a complete CRUD for invoices, so you could use it to develop an alternative client (such as a mobile app).

CryptoInvoice doesn't hold any private keys. It uses an XPUB (extended public key) to generate addresses for target user without knowing their private keys. We support all three address derivation standards: BIP 32, BIP 44 and BIP 49. The derivation path is chosen based on the XPUB format. 32 and 44 are compatible, and their XPUB has `xpub` prefix. BIP 49 (used by Trezor hardware wallets) has the `ypub` prefix. Both are supported in the UI.

### Architecture diagram

![Architecture diagram](https://raw.githubusercontent.com/simplifate/crypto-invoice/master/wiki-assets/cryptoinvoice-architektura-v2.png)
