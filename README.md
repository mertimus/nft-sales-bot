# Solana NFT Sales Bot
A short sales bot/script for tracking NFT sales on Solana for a given collection and posting the sale details to Discord.

The code here isn't great and is primarily meant for illustrating concepts. Please see [this accompanying post](https://mertmumtaz.medium.com/building-an-nft-sales-bot-with-javascript-and-solana-3d7add28f995) which explains all parts of the bot.

# Usage
Once you've got a project address and a Discord webhook URL, simply run:

`PROJECT_ADDRESS=insert-address DISCORD_URL=insert-webhook node sales_bot.js`

# Caveats
The way the bot is currently set up, it fetches the last 1000 signatures by default. This is on purpose - as I like backfilling historic sales when I add a bot to Discord.

If you do NOT want to do this, i.e., if you want to ONLY trigger the bot for NEW sales, you will need to modify the code.

There are a few ways to do this.

You can simply call `getSignaturesForAddress` before booting up the bot to get the most recent transaction and then pass this signature to the `getSignaturesForAddress` in the `until` option. 

For example: 

```
  const runSalesBot = async () => {
    const mostRecentSignature = await solanaConnection.getSignaturesForAddress(projectPubKey, { limit: 1 });
    const options = { until: mostRecentSignature[0].signature }
 
    // ... more stuff
    
    while(true) {
      let signatures = await solanaConnection.getSignaturesForAddress(projectPubKey, options);
      // ... rest of the code
    }
```

Alternatively, you can set a new Date during bootup and make sure that new sales occured after this date.

# Issues
Sometimes there are problems with the Metaplex API, if this happens, please use the Magic Eden API for getting the metadata (I go over this in the Medium post), i.e., `https://api-mainnet.magiceden.io/rpc/getNFTByMintAddress/{paste-mint-address-here`
