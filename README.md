# buildspace Solana NFT Drop Project
### Welcome 👋
To get started with this course, clone this repo and follow these commands:

1. cd into the `app` folder
2. Run `npm install` at the root of your directory
3. Run `npm run start` to start the project
4. Start coding!

### What is the .vscode Folder?
If you use VSCode to build your app, we included a list of suggested extensions that will help you build this project! Once you open this project in VSCode, you will see a popup asking if you want to download the recommended extensions :).

## Development
### 1. Deploy your NFTs to Solana's Devnet using `arweave` storage
#### o. create `assets` folder in _~/root_ of project

For each asset create 2 files: **0.png** and **0.json**  <br>
Simple `.json` with creator wallet address:
```
{
  "name": "NAME_OF_NFT",
  "symbol": "",
  "image": "0.png",
  "properties": {
    "files": [
      {
        "uri": "0.png",
        "type": "image/png"
      }
    ],
    "creators": [
      {
        "address": "INSERT_YOUR_WALLET_ADDRESS_HERE",
        "share": 100
      }
    ]
  }
}
```

Be sure to change certain attributes for each `.json`: `name`, `image`, `uri`.
Set the `address` by your own preferences.

#### a. Setting up a Solana keypair
```
solana-keygen new --outfile ~/.config/solana/devnet.json
solana config set --keypair ~/.config/solana/devnet.json
```

> run `solana balance` to get the local solana wallet balance <br>
> run `solana airdrop 2` to gain 2 SOL for developing purposes

#### b. Configure your candy machine
Create new file `config.json` in project ___~/root___ .
> Important to change: `price`, `number`, `solTreasuryAccount`, `goLiveDate`, `storage` <br>
> Note: default storage is **`arweave`**

#### c. Upload the NFTs and create your candy machine
```
ts-node ~/metaplex/js/packages/cli/src/candy-machine-v2-cli.ts upload -e devnet -k ~/.config/solana/devnet.json -cp config.json ./assets
```

#### d. Verify NFTs
```
ts-node ~/metaplex/js/packages/cli/src/candy-machine-v2-cli.ts verify_upload -e devnet -k ~/.config/solana/devnet.json
```

#### e[dition]. Update candy machine config 

If needed, update the `config.json` and run this command
```
ts-node ~/metaplex/js/packages/cli/src/candy-machine-v2-cli.ts update_candy_machine -e devnet -k ~/.config/solana/devnet.json -cp config.json
```

### 2. Upgrade your NFTs with IPFS
1. Go to Pinata and create New API Key
Make sure `pinFileToIPFS` access is enabled
2. Copy everything from popup
Need to copy `API Key`, `API Secret`, `JWT`.
3. Update `config.json` file
Change: `ipfs`, `ipfsInfuraProjectId`, `ipfsInfuraSecret`. <br>
Add new attributes: `pinataJwt`, `pinataGateway`.
```
{
  "price": 0.1,
  "number": 3,
  "gatekeeper": null,
  "solTreasuryAccount": "<YOUR WALLET ADDRESS>",
  "splTokenAccount": null,
  "splToken": null,
  "goLiveDate": "01 Apr 2022 00:00:00 GMT",
  "endSettings": null,
  "whitelistMintSettings": null,
  "hiddenSettings": null,
  "storage": "ipfs",
  "pinataJwt": "<YOUR PINATA JWT TOKEN>",
  "pinataGateway": "null",
  "ipfsInfuraProjectId": "<YOUR PINATA API KEY>",
  "ipfsInfuraSecret": "<YOUR PINATA API SECRET>",
  "awsS3Bucket": null,
  "noRetainAuthority": false,
  "noMutable": false
}
```
4. Upload to IPFS
Delete your `.cache` folder (if exist) and run the upload command again:
```
ts-node ~/metaplex/js/packages/cli/src/candy-machine-v2-cli.ts upload -e devnet -k ~/.config/solana/devnet.json -cp config.json ./assets
```
