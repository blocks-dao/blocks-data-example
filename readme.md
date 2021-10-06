# BLOCKS For Data Transactions

BLOCKS is an ERC777 token which is compatible with ERC20. One unique feature of BLOCKS is the ability to insert data into a transaction using the "send" function.

# Use Cases for BLOCKS For Data Transactions

- Inserting hashes for data integrity management
- Sending human-readable messages
- Storing Immutable NFT metadata

## BLOCKS Contract Address

[0x8a6D4C8735371EBAF8874fBd518b56Edd66024eB](https://ropsten.etherscan.io/token/0x8a6D4C8735371EBAF8874fBd518b56Edd66024eB)

## BLOCKS ABI

[See here](/blocksAbi.json)

## Usage

You can connect to the BLOCKS contract using [web3.js](https://www.npmjs.com/package/web3). The following exampled will show you how to insert data into a transaction, and also fetch then parse the data from the blockchain.

### Import Essential Libraries

Common libraries used for working with Ethereum Smart Contracts.

[ethers.js](https://www.npmjs.com/package/ethers)
[web3 Utils](https://www.npmjs.com/package/web3-utils)
[web3.js](https://www.npmjs.com/package/web3)
[bignumber.js](https://www.npmjs.com/package/bignumber.js)



### Adding Data to a Transaction

This example uses Metamask as the Ethereum Provider.

```
const blocksDataTransaction = () => {

      //Connect to Ethereum through the Metamask Provider
      window.ethereum.enable().then(provider = new ethers.providers.Web3Provider(window.ethereum));
      const signer = provider.getSigner();

      //Connect to the BLOCKS Smart Contract via the contract address, abi and provider
      const contract = new ethers.Contract(environment.blocksContract, environment.blocksAbi, provider);
      let contractSigner = contract.connect(signer);

      //Define the data you want to insert on-chain
      let data = "Hello, this message is going on-chain!";
      let dataConverted = web3utils.toHex(data);

      //You can send an amount of BLOCKS tokens with the transaction. BigNumber helps JavaScript deal with large numbers involving BLOCKS 18 decimals. In this case we are sending 2 BLOCKS.
      let amount = new BigNumber(2000000000000000000);

      //Now you can call the "send" function by entering a receiving address, amount and the converted data.
      let receivingAddress = "0xA3Ccd256639c67639f39e69FDDd46Ac4230CD886"
      contractSigner.send(receivingAddress, amount.toFixed(), dataConverted).then((tx: any)=>{
        if(tx){
          //View the transaction response and get the transaction hash
          console.log(tx)
          alert(tx.hash);
        }
      }).catch((e: any) => {
        alert(e);
      });
}
```

### Fetching and Parsing BLOCKS Data From a Transaction

For fetching data we need to connect to an Ethereum provider like [Infura](https://infura.io/)

```
const fetchBlocksTransaction = async() => {

    //Connect web3 to Ethereum provider
    let web3 = new Web3('https://mainnet.infura.io/v3/<infura-api-key>');

    //Fetch the transaction id using web3. Insert the transaction id that contains BLOCKS data.
    web3.eth.getTransactionReceipt('0x30e2c4a17d5622364f7a66062e56918e8f11d73f814ae11ee0504b27c3f3fd1b').then(res => {

      //Call a function to decode the transaction logs, see below.
      getLogData(res).then(result => {
        console.log(result.data)

        //Use web3 Utils to convert the hex data to human-readable data.
        let message = web3Utils.hexToAscii(result.data);
        alert(message);
      });
    });
  }

  const getLogData = (decode) => {

      //We need to decode the transaction logs to get the data. This requires the abi of the "send" function, the data and transaction topics. These are passed in as a result of fetching the transaction.
      let web3 = new Web3('https://mainnet.infura.io/v3/<infura-api-key>');
      let result = web3.eth.abi.decodeLog([{
        'type':'address',
        'name':'operator',
        'indexed': true
      }, {
        'type':'address',
        'name':'from',
        'indexed': true
      },{
        'type':'address',
        'name': 'to',
        'indexed': true
      },{
        'type': 'uint256',
        'name':'amount'
      },{
        'type': 'bytes',
        'name':'data'
      },{
        'type': 'bytes',
        'name':'operatorData'
      }],decode.logs[0].data, decode.logs[0].topics.slice(1));
      return result;
  }
```

## Have Fun

Have fun exploring and building on BLOCKS with data transactions.


## License

Copyright © 2021 <BLOCKS DAO, LLC>

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.