# Harnessing Lighthouse Network for NFT Minting: A Developer's Guide

![Lighthouse Image](https://example.com/lighthouse-image.jpg)

The world of Non-Fungible Tokens (NFTs) has revolutionized the digital realm, offering unique opportunities for creators and enthusiasts. Entering this domain can be challenging, especially when integrating with blockchain technology. This guide simplifies the process, detailing how to create and manage NFTs using the Astar Network EVM and Lighthouse Storage while leveraging the XCM protocol communication.   

## Step 1: Setting Up an EVM Wallet
Before crafting your NFT, ensure you have a Web3 wallet ready. This wallet bridges traditional web browsers with the Ethereum blockchain, a renowned platform for NFTs. Popular choices include Metamask, Talisman, and Subwallet.
- Start by connecting your Web3 wallet.

```javascript
async connect() {
  const connect = await this.onboard.connectWallet();
  this.wallet = await this.onboard.connectedWallet;

  if (this.wallet) {
    this.provider = new ethers.providers.Web3Provider(this.wallet.provider);
    this.signer = this.provider.getSigner();
    this.address = await this.signer.getAddress();
    this.set();
  }

  return { connect };
}
```

For a comprehensive understanding of the onboard wallet provider, refer to [this article](https://astar.network/blog/one-small-piece-of-code-one-giant-leap-for-web3-37760/).

## Step 2: Digital Signatures
With a connection established, you'll need to sign a message. This signature is crucial for Lighthouse's API to authenticate your EVM address. Consider it your digital stamp, verifying your blockchain identity.
- Click the "Sign" button to authenticate.

```javascript
async sign() {
  this.sig = await this.signer.signMessage(this.address);
}
```

## Step 3: Uploading to Lighthouse - Your First Major Move
With your signature ready, it's time to upload your image and metadata to the Lighthouse network. This decentralized platform ensures your data's safety and accessibility.
- Click the "Upload" button to begin.

```javascript
import * as dotenv from 'dotenv';
dotenv.config();
import lighthouse from '@lighthouse-web3/sdk';

const uploadFile = async () => {
  const path = "path/to/your/file"; // Provide the path to the file
  const apiKey = process.env.API_KEY; 
  // Generate the API key from https://files.lighthouse.storage/ 
  //or using CLI (lighthouse-web3 api-key --new)

  // Both files and folders are supported by the upload function
  const response = await lighthouse.upload(path, apiKey);

  console.log(response);
  console.log("Visit at: https://gateway.lighthouse.storage/ipfs/" + response.data.Hash);
  return `https://gateway.lighthouse.storage/ipfs/${response.data.Hash}`
}

let tokenURI = uploadFile();

```

## Step 4: Minting Your NFT
With your data pinned, you're set to mint your NFT. This step transforms your digital asset into a unique, blockchain-certified NFT.
- Click the "Mint" button to finalize.

```javascript
import * as dotenv from 'dotenv';
dotenv.config();

const abi = [....] //your contract ABI

const contractAddress = process.env.CONTRACT_ADDRESS; 
const privateKey = process.env.PRIVATE_KEY;

async mint() {
  // Get signer and provider
  const provider = new ethers.JsonRpcProvider();
  const signer = new ethers.Wallet(privateKey, provider);

  const contract = new ethers.Contract(
    contractAddress,
    abi, //write a smart contract to handle minting of NFT
    signer
  );

  // Mint the NFT
  const txResponse = await contract.mintNFT(this.tokenURI);
  const txReceipt = await txResponse.wait();
  const [transferEvent] = txReceipt.events;
  const { tokenId } = transferEvent.args;
  console.log("NFT minted successfully!");
  console.log(`NFT tokenId: ${tokenId}`);

  const tokenURIonchain = await contract.tokenURI(tokenId);
  console.log("tokenURI", tokenURIonchain);
}
```

Congratulations on minting your NFT! 

## Conclusion
This guide offers a step-by-step introduction to NFT creation and management using the Astar Network EVM and Lighthouse Network. As you familiarize yourself with these steps, you'll be better equipped to delve deeper into the dynamic world of NFTs. Welcome to the digital frontier!

---