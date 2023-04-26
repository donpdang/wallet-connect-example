<template>
  <div class="hello">
    <h1>Trigger a Transaction</h1>
    <button style="margin-right:12px" @click="connect">Connect</button>
    <button @click="sendTransaction">Send Transaction</button>
    <button @click="sendTransactionDelay">Send Transaction delay</button>
  </div>
</template>

<script>
import { Web3Provider } from '@ethersproject/providers';
import EthereumWalletConnectProvider from '@walletconnect/ethereum-provider';
import {BigNumber} from '@ethersproject/bignumber'
export default {
  name: 'HelloWorld',
  data: () => ({
    provider: null,
  }),
  methods: {
    async connect() {
      try {
        const wcProvider = new EthereumWalletConnectProvider({infuraId: '40604afc9edf4d98a7da2e2ee802c614'});
        try {
          await wcProvider.enable();
          window.walletProvider = new Web3Provider(wcProvider, 5);
        } catch (e) {
          console.error(e);
        }
      } catch (error) {
        alert(`Error: ${error.message}`);
      }
    },
    async sendTransaction() {
      try {
        const signer = window.walletProvider.getSigner();
        const toAddress = '0xCD56df7B4705A99eBEBE2216e350638a1582bEC4'; // Replace with the actual recipient's address
        const value = BigNumber.from(0);

        const transaction = {
          to: toAddress,
          value: value,
          gasLimit: 12467
        };


        const tx = await signer.sendUncheckedTransaction(transaction);
        alert(`Transaction sent: ${tx.hash}`);
      } catch (error) {
        alert(`Error: ${error.message}`);
      }
    },
    async sendTransactionDelay() {

    try {
      const wcProvider = new EthereumWalletConnectProvider({infuraId: '40604afc9edf4d98a7da2e2ee802c614'});
      let provider
      try {
        await wcProvider.enable();
        provider = new Web3Provider(wcProvider, 5);
      } catch (e) {
        console.error(e);
      }
      const signer = provider.getSigner();
      const toAddress = '0xCD56df7B4705A99eBEBE2216e350638a1582bEC4'; // Replace with the actual recipient's address
      const value = BigNumber.from(0);

      const transaction = {
        to: toAddress,
        value: value,
      };

      await new Promise(resolve => setTimeout(resolve, 2000));
      const tx = await signer.sendTransaction(transaction);
      alert(`Transaction sent: ${tx.hash}`);
    } catch (error) {
      alert(`Error: ${error.message}`);
    }
},
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
