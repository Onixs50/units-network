# units-network
go here 
[unitnetwork](https://app.units.network/?referral=0x975B78F5d585df460161D27E0434F9aD92aC1932)
get faucet 
[faucet](https://faucet-testnet.unit0.dev/)
#  Node.js
```sh
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```
```sh
npm install -g npm@10.8.1
npm install web3@1.5.3
```
```sh
nano unit.js
```
# paste this code 
```sh
const readline = require('readline');
const Web3 = require('web3');
const fs = require('fs');

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

const rpcURL = "https://rpc-testnet.unit0.dev";
const web3 = new Web3(new Web3.providers.HttpProvider(rpcURL));

function getRandomAmount(min, max) {
  return (Math.random() * (max - min) + min).toFixed(8);
}

function getRandomInterval(min, max) {
  return Math.floor(Math.random() * (max - min + 1) + min);
}

async function startTransactions() {
  console.log('\x1b[36m%s\x1b[0m', 'Coded By Onixia');
  
  const privateKeys = fs.readFileSync('private_keys.txt', 'utf-8').split('\n').filter(key => key.trim() !== '');

  const amountRange = await new Promise((resolve) => {
    rl.question('\x1b[33mEnter a "range" of amounts to transfer (e.g., 0.0000001,0.00002): \x1b[0m', (range) => {
      resolve(range);
    });
  });

  const [minAmount, maxAmount] = amountRange.split(',').map(Number);

  while (true) {
    for (const privateKey of privateKeys) {
      const account = web3.eth.accounts.privateKeyToAccount(privateKey);
      web3.eth.accounts.wallet.add(account);
      web3.eth.defaultAccount = account.address;

      console.log(`\x1b[32mWallet Address: ${account.address}\x1b[0m`);

      for (let i = 0; i < 10; i++) {
        const toAccount = web3.eth.accounts.create();
        fs.appendFileSync('recipient_addresses.txt', `${toAccount.address}\n`);

        const amount = getRandomAmount(minAmount, maxAmount);
        const interval = getRandomInterval(8, 12);

        const tx = {
          from: web3.eth.defaultAccount,
          to: toAccount.address,
          value: web3.utils.toWei(amount, 'ether'),
          gas: 21000,
          gasPrice: web3.utils.toWei('1', 'gwei'),
          chainId: 88817
        };

        try {
          const receipt = await web3.eth.sendTransaction(tx);
          console.log(`\x1b[34mTransaction successful with hash: ${receipt.transactionHash} | Amount sent: ${amount} UNIT0 | Next transaction in: ${interval} seconds\x1b[0m`);
        } catch (err) {
          console.error('\x1b[31mError sending transaction:', err, '\x1b[0m');
          break;
        }

        await new Promise(resolve => setTimeout(resolve, interval * 1000));
      }
    }
  }
}

startTransactions();
```
##  paste your private in( private_keys.txt)
```sh
nano private_keys.txt
```
```sh
screen -S unit
```
# run it 
```sh
node unit.js
