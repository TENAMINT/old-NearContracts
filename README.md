# TokenizedCard Token (NFT)

## 1. TokenizedCard - Just a normal NFT contract with metadata. Should show up in the NEAR wallet (NFT tab)

Constructor params

- `costPerToken` (in `$USN`)
- `totalSupply`
- `metadata` - NFT metadata
- `owner` - account to receive the $USN

Methods

- `buy` - will transfer the maximum available number of associated TokenizerCard NFTs that is fully covered by the $USN amount the user has sent. Should send the charged $USN to the owner, and refund the remaining $USN

  - _Example1:_ Card costs $USN 10, user calls buy and sends $USN 43, there are 100 card tokens available => user gets 4 card tokens, and is refunded $USN 3.

  - _Example 2:_ Card costs $USN 21, user calls buy and sends $USN 163, there are 3 card tokens available => user gets 3 card tokens, and is refunded $USN 100.

## 2. CardStorefront - Deploys a new TokenizerCard with the following config

`POST /deployStorefront`

```json
{
  "costPerToken": 1,
  "totalSupply": 100,
  "metadata": {}
}
```

## 3. CardMarketplace

Constructor params

- fee - percentage to take as fee from every 'buy' transaction
- feeRecipient - where to transfer the fee

Methods

- whitelist

  - `allowlist_card` (addr tokenizedCard, uint minPricePerToken)

    Whitelists a TokenizedCard smart contract for listing on the 2ndary marketplace

  - `disallow_card` (addr tokenizedCard)

    Removes a TokenizedCard from the whitelist. All listed cards of said type are immediately unlisted from the marketplace.

- sale

  - `list` (addr tokenizedCard, string tokenId, uint price)

    List a specific NFT for sale

  - `unlist` (addr tokenizedCard, string tokenId)

    Removes a specific NFT listed by the user from the marketplace

  - `buy` (addr tokenizedCard, string tokenId)

    Will transfer the TokenizerCard NFT if listed by its owner for the $USN amount the user has sent or less. Should send the charged listed price amount (minus the fee) of $USN to the previous owner of the acquired cards, the fee to the feeRecipient, and refund the remaining $USN

## 4. Sub Wallet && Fungible Token

- Create a Node.js Express `/new-wallet/:uid` endpoint. It should create a new NEAR wallet with the name `$uid.tenamint-wallet.near`.

- Create a another Node.js Express `/buy-card/:uid/:nft-id` endpoint to be used for allowing buying cards offchain.

- Create a fungible token - `tenamint-iou-usn.near`. We’ll use this token together with the endpoint 2 to keep track of Stripe linked payments.
