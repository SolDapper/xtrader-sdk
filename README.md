# xtrader-sdk
Javascript SDK for the xTrader Blockchain Program on Solana

![powered by solana](https://cd6na2lma222gpigviqcpr5n7uewgxd7uhockofelflsuaop7oiq.arweave.net/EPzQaWwGtaM9BqogJ8et_QljXH-h3CU4pFlXKgHP-5E)

# xTrader Blockchain Program
[xtrader program](https://solana.fm/address/BG9YVprV4XeQR15puwwaWfBBPzamTtuMRJLkAa8pG5hz/verification)

# Install SDK
```javascript
npm i xtrader-sdk
```

# Import
```javascript
import xtrader from 'xtrader-sdk';
```

# Methods
```javascript
xtrader.Create
xtrader.Execute
xtrader.Cancel
xtrader.Received
xtrader.Sent
```
```javascript
xtrader.Find
xtrader.Fetch
xtrader.Fee
xtrader.Send
xtrader.Status
xtrader.Sns
```

# Examples

## Setup Example
```javascript
import { Keypair } from "@solana/web3.js";
const rpc = "https://staked.helius-rpc.com?api-key=YOUR-KEY";
const secret = [1,2,3,4,5,~];
const signer = Keypair.fromSecretKey(new Uint8Array(secret));
```

### Create Offer
```javascript
const tx = await xtrader.Create({
    rpc: rpc,
    priority: "Medium", // priority fee level
    convert: true, // true because we're passing decimal values below
    seller: "7Z3LJB2rxV4LiRBwgwTcufAWxnFTVJpcoCMiCo8Z5Ere",
    token1Mint: "Xsc9qvGR1efVDFGLrVsmkzv3qi45LTBjeUKSPmx9qEh",
    token1Amount: "0.001",
    buyer: "B8owyFUUu46g8Z4JNZMXmLSc2D725zv6fcXuBewGeTyj",
    token2Mint: "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
    token2Amount: "0.007",
    memo: "", // optional reference also applied to accepted offer tx
});
if(tx.tx){
    tx.tx.sign([signer]);
    const signature = await xtrader.Send(rpc,tx.tx);
    console.log("signature", signature);
    console.log("awaiting status...");
    const status = await xtrader.Status(rpc,signature);
    if(status!="finalized"){console.log(status);}
    else{
        console.log(status);
        const offer = await xtrader.Fetch({
            rpc:rpc, display:true, offer:tx.offer
        });
        console.log(offer);
    }
}
else{
    console.log(tx);
}
```

### Cancel Offer
```javascript
const tx = await xtrader.Cancel({
    rpc: rpc,
    offer: "2jcih7dUFmEQfMUXQQnL2Fkq9zMqj4jwpHqvRVe3gGLL" // offer id
});
if(typeof tx.status!="undefined"){console.log(tx);}
else{
    tx.sign([signer]);
    const signature = await xtrader.Send(rpc,tx);
    console.log("signature", signature);
    console.log("awaiting status...");
    const status = await xtrader.Status(rpc,signature);
    console.log(status);
}
```

### Execute Offer
```javascript
const tx = await xtrader.Execute({
    rpc: rpc,
    offer: "3pjxfm25WWwD9BcWSqBFamJKYgEpNAnEz8mEmxk9biBQ",
    buyer: "B8owyFUUu46g8Z4JNZMXmLSc2D725zv6fcXuBewGeTyj"
});
if(typeof tx.status!="undefined"){console.log(tx);}
else{
    tx.sign([signer]);
    const signature = await xtrader.Send(rpc,tx);
    console.log("signature", signature);
    console.log("awaiting status...");
    const status = await xtrader.Status(rpc,signature);
    console.log(status);
}
```

### Received Offers
```javascript
const received = await xtrader.Received({
    rpc: rpc,
    display: true,
    wallet: "B8owyFUUu46g8Z4JNZMXmLSc2D725zv6fcXuBewGeTyj"
});
console.log(received);
```

### Sent Offers
```javascript
const sent = await xtrader.Sent({
    rpc: rpc,
    display: true,
    wallet: "7Z3LJB2rxV4LiRBwgwTcufAWxnFTVJpcoCMiCo8Z5Ere"
});
console.log(sent);
```

### Fetch Offer Details
returns an offers's details
```javascript
const offer = await xtrader.Fetch({
    rpc: rpc,
    display: true,
    offer: "DUjEPTHQsUizXcyfix5iEnxvU6vMxU6EJW4FEHs9Xgrb",
});
console.log(offer);
```

### Get Base Fee
get the base offer fee
```javascript
const fee = await xtrader.Fee({
    rpc: rpc, 
    display: true, // true = sol, false = lamports
});
console.log(fee);
```