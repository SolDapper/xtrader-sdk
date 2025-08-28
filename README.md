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
    builder: true, // builder false will return ix for tx only
    blink: false, // blink true will return a base64 formatted object
    tolerance: 1.2, // cu estimate multiplier for padding if needed
    priority: "Medium", // priority fee level
    convert: true, // convert true because we're passing decimal values for amounts below
    affiliateWallet: "ACgZcmgmAnMDxXxZUo9Zwg2PS6WQLXy63JnnLmJFYxZZ",
    affiliateFee: "0.0009",
    seller: "7Z3LJB2rxV4LiRBwgwTcufAWxnFTVJpcoCMiCo8Z5Ere", // provider
    token1Mint: "So11111111111111111111111111111111111111112",
    token1Amount: "0.001",
    token2Mint: false,
    token2Amount: false,
    buyer: false, // buyer false makes this a public listing
    physical: 0, // 0 = Digital, 1 = Phygital + Shipping, 2 = Phygital Pick-Up, 
    memo: "", // optional title or reference number
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
    escrow: "2jcih7dUFmEQfMUXQQnL2Fkq9zMqj4jwpHqvRVe3gGLL" // escrow id (acct)
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
    convert: true,
    affiliateWallet: "ACgZcmgmAnMDxXxZUo9Zwg2PS6WQLXy63JnnLmJFYxZZ",
    affiliateFee: "0.0009",
    offer: "3pjxfm25WWwD9BcWSqBFamJKYgEpNAnEz8mEmxk9biBQ",
    buyer: "2jcih7dUFmEQfMUXQQnL2Fkq9zMqj4jwpHqvRVe3gGLL"
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
    wallet: "2jcih7dUFmEQfMUXQQnL2Fkq9zMqj4jwpHqvRVe3gGLL"
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

### Sns
used internally, accepts the data object passed to the parent method
returns the object with seller and buyer .sol addresses converted
```javascript
_data_ = await xtrader.Sns(connection, _data_);
```

### Find Offer Id
returns an offer id or false
```javascript
const offer = await xtrader.Find({
    rpc: rpc,
    seller: "7Z3LJB2rxV4LiRBwgwTcufAWxnFTVJpcoCMiCo8Z5Ere",
    mint: "35rxoAdMJXm6cSSEpQ25qgmLFjhShpsEMc3guQjeZva8",
});
console.log(offer);
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