Instalation guide
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

1 - Clone this repository  
2 - Run `git submodule init` and `git submodule update`  
3 - Set connection to Sentry in `blockchain-observer/BlockchainObserver/appsettings.json` and `blockchain-observer/BlockchainObserver/appsettings.json`  
4 - Start a litecoind node: https://github.com/litecoin-project/litecoin  
5 - Set the credentials for the JSON RPC API of your Litecoin node in `blockchain-observer/BlockchainObserver/appsettings.json`.  
6 - Set your IP and port where WebAPI will run in `frontend/src/appSettings.json`  
7 - Open the RabbitMQ administration UI and Create 3 queues: `BTC`, `LTC` and `WebAPI`.  
8 - In the WebDAV container volume, create directory `/media/invoices`  
9 - Run Entity Framework migrations on WebAPI and blockchain_observer. This will create the DB scheme.  
10 – Restart docker compose, open the UI, and register a user account.  

### For UI development

1 - Install Node.js and NPM - https://nodejs.org  
2 - Go to the frontend repository of CryptoInvoice  
3 - Set your IP and port in `frontend/src/appSettings.json` to a running WebAPI instance (change the development settings, not   production)  
4 - Run `npm install` - this will install all frontend dependencies.  
5 - Run `npm run dev` - this will start the webpack dev server for frontend on your localhost.  

### For extending WebAPI or Blockchain Observer

Both are written in C# / .NET Core 2.0, which is cross platform. You can use Visual Studio Code to develop .NET Core apps on Linux, Mac and Windows - https://code.visualstudio.com/

### Other

If you would like to develop an alternative blockchain observer for another currency, all you need to do is connect your service to RabbitMQ and communicate using JSON RPC notifications. A specification of the API is in the **wiki** for this repository.

The wiki also contains documentation for the REST API used by frontend. It has a complete CRUD for invoices, so you could use it to develop an alternative client (such as a mobile app).

### Architecture diagram

![Architecture diagram](https://raw.githubusercontent.com/simplifate/crypto-invoice/master/wiki-assets/cryptoinvoice-architektura-v2.png)
