#Acala API测试例程

例程展示了生成钱包地址、查询地址信息和发交易（订阅交易事件）的过程

package.json

```
{
  "name": "polkadot-test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node index.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@acala-network/api": "^0.4.0-beta.33",
    "@polkadot/api": "^2.3.2-17",
    "@polkadot/rpc-provider": "^2.10.1"
  }
}
```

index.js

```
const { ApiPromise, Keyring } = require('@polkadot/api');
const { WsProvider } = require('@polkadot/rpc-provider');
const { options } = require('@acala-network/api');

async function main () {
  // Mnemonic phrase
  const PHRASE = 'elegant protect garbage like emerge nest pear insect demand they volcano mention';
  // Receiver
  const BOB = '5FjYcMXoTVA3pRfnHC6h5ZLNQyWDa5Cu3TeuJNU99Dq8iGnz';

  const provider = new WsProvider(['wss://node-6714447553777491968.jm.onfinality.io/ws']);
  const api = new ApiPromise(options({ provider }));

  await api.isReady;
  console.log(api.genesisHash.toHex());
  const data = await api.query.system.account(BOB);

  console.log(data.toHuman());

  const keyring = new Keyring({ type: 'sr25519' });

  // Add an account, straight mnemonic
  const alice = keyring.addFromUri(PHRASE);

  console.log('alice: ', alice.toJson());
  console.log('alice.address: ', alice.address);

  // Create a extrinsic, transferring 12345 units to Bob
  const transfer = api.tx.balances.transfer(BOB, 12345);

  // Retrieve the payment info
  const { partialFee, weight } = await transfer.paymentInfo(alice);

  console.log(`transaction will have a weight of ${weight}, with ${partialFee.toHuman()} weight fees`);

  // Sign and send the transaction using our account
  const txHash = await transfer.signAndSend(alice);

  console.log(`Submitted with hash ${txHash}`);

  // Transaction subscriptions
  // const unsub = await transfer.signAndSend(alice, ({ events = [], status }) => {
  //   console.log(`Current status is ${status.type}`);

  //   if (status.isInBlock) {
  //     console.log(`Transaction included at blockHash ${status.asInBlock}`);
  //   } else if (status.isFinalized) {
  //     console.log(`Transaction finalized at blockHash ${status.asFinalized}`);
  //     events.forEach(({ event: { data, method, section }, phase }) => {
  //       console.log(`\t' ${phase}: ${section}.${method}:: ${data}`);
  //     });
  //     unsub();
  //   }
  // });
}

main();

```



查询transaction信息

```
curl --location --request POST 'https://acala-testnet.subscan.io/api/scan/extrinsic' \
--header 'Content-Type: application/json' \
--data-raw '{
    "hash": "0xa77141d36ea177d6ba8cc0a00cdf3db14844925938aad441b4b26d128937f32d"
}' 
```



参考资料

1. polkadot.js文档

   https://polkadot.js.org/docs/api/start/api.tx

2. subscan文档

   https://docs.api.subscan.io/#extrinsic

3. Substrate 前端开发-1: 用 Polkadot-JS API 轻松搭建前端

   https://zhuanlan.zhihu.com/p/110811972