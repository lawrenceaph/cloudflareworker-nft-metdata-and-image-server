# A Metadata Provider That Only Serves Data for Minted Tokens

This is a cloudflare workers project that takes in requests for NFT metadata and images. 

It protects the metadata (and true file location) of un-minted tokens by only serving JSON files pertaining to minted NFTs. It also gates access to images pertaining to un-minted NFTs by.

It's easier to understand what it does by visiting [the deployed worker itself](https://wart.unstoppable.gallery/1.json) and viewing a JSON file. 

Aside from serving JSON files for metadata, the worker also serves the images pointed to by the metadata. 

Below image was originally obtained from: wart.unstoppable.gallery/4.png.


![image from worker](https://wart.unstoppable.gallery/4.png)


**How it works:**

Let's assume that your smart contract points to the url "https://wart.unstoppable.gallery/", followed by "1.json" as the token URI for token ID number 1. 

The worker will take that requested url, pull the metadata from the IPFS folder (while masking the CID), and return the JSON file to the client making the request. 

The default location of the metadata in this project exists, as does the contract on polygon. There is a flaw intentionally placed into one file (the file served when the contract metadata state is "hidden"), so that we can demonstrate the vulnerabilities introduced by carelessly leaving a CID in plain view. 

Until there is a release version 1.0 on the right side of the github repo, please consider this a preview version that may contain bugs. 

It is designed to work well with contracts such as the Hashlips Lab ERC 721 NFT contract, as well as any contract that uses the ERC 721 standard. 

Enjoy!

Instructions:
1. Run NPM install after cloning this repository.
2. Create a cloudflare account and run wrangler login to authorize the CLI.
3. Publish the worker with wrangler publish and visit the worker to test if everything is behaving properly.
4. If the worker is responding to your request / visit, proceed to tweak the code to match your project details.
5. You can use a public endpoint to make sure you aren't revealing sensitive data in your repository, but you can also use environment variables through cloudflare secrets, to keep data safe.
6. You can edit the token's metdata URI (line 66 of index.js) and image URI (line 83 of index.js) to suit your use case. 
7. Change your smart contract's token URI prefix to the new URL (the worker URL)
8. Run a local test with wrangler dev.
9. Correct any errors that come up.
10. Publish your worker when ready! :)
