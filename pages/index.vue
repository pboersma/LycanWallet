<template>
  <div class="container mx-auto mt-20">
    <h1 class="text-6xl">{{ message }}</h1>
    <p>Is Ethereum Supported : {{ ethereumSupported }}</p>
    <h1 class="text-4xl text-green-600">Current Balance : {{ currentBalance }}</h1>
    <button @click="contractConnect">Get Balance</button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  data() {
    const message: string = 'Hello Vue!'
    const contractAddress: string = '0x574c898F27207c53800c7668dd42CA6E1e9D3f38'
    const ethereumSupported: any = false
    const currentBalance: any = null;

    return {
      message,
      contractAddress,
      ethereumSupported,
      currentBalance
    }
  },
  async mounted() {
    this.ethereumSupported = await this.isEthereumSupported()
  },
  methods: {
    async isEthereumSupported() {
      if ((window as any).ethereum) {
        // (window as any).web3 = new this.$Web3((window as any).ethereum);
        ;(window as any).web3 = new this.$Web3((window as any).ethereum)
        try {
          // Request account access
          await (window as any).ethereum.enable()
          return true
        } catch (error) {
          return false
        }
      }
      // Non-decentralized app browsers...
      else {
        console.log(
          'Non-Ethereum browser detected. You should consider trying MetaMask!'
        )
      }
    },
    async contractConnect() {
      const self = this;
      const currentContract = await new (window as any).web3.eth.Contract(
        [
          {
            inputs: [],
            payable: false,
            stateMutability: 'nonpayable',
            type: 'constructor',
          },
          {
            anonymous: false,
            inputs: [
              {
                indexed: true,
                internalType: 'address',
                name: '_from',
                type: 'address',
              },
              {
                indexed: true,
                internalType: 'address',
                name: '_to',
                type: 'address',
              },
              {
                indexed: false,
                internalType: 'uint256',
                name: '_value',
                type: 'uint256',
              },
            ],
            name: 'Transfer',
            type: 'event',
          },
          {
            constant: false,
            inputs: [
              {
                internalType: 'address',
                name: 'receiver',
                type: 'address',
              },
              {
                internalType: 'uint256',
                name: 'amount',
                type: 'uint256',
              },
            ],
            name: 'sendCoin',
            outputs: [
              {
                internalType: 'bool',
                name: 'sufficient',
                type: 'bool',
              },
            ],
            payable: false,
            stateMutability: 'nonpayable',
            type: 'function',
          },
          {
            constant: true,
            inputs: [
              {
                internalType: 'address',
                name: 'addr',
                type: 'address',
              },
            ],
            name: 'getBalanceInEth',
            outputs: [
              {
                internalType: 'uint256',
                name: '',
                type: 'uint256',
              },
            ],
            payable: false,
            stateMutability: 'view',
            type: 'function',
          },
          {
            constant: true,
            inputs: [
              {
                internalType: 'address',
                name: 'addr',
                type: 'address',
              },
            ],
            name: 'getBalance',
            outputs: [
              {
                internalType: 'uint256',
                name: '',
                type: 'uint256',
              },
            ],
            payable: false,
            stateMutability: 'view',
            type: 'function',
          },
        ],
        this.contractAddress
      )

        currentContract.methods.getBalance("0xabB4E2EE963c6cD519d00387Dfffd8aD77a79B87").call()
        .then(function(result: any){
            self.currentBalance = result;
        });

    },
  },
})
</script>
