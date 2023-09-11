<script setup>
import { useOnboard } from '@web3-onboard/vue'

import { ethers } from 'ethers'
import { watch } from 'vue';

const { connectedWallet, connectedChain, connectingWallet, connectWallet, disconnectWallet } =
  useOnboard()

if (connectedWallet?.provider) {
  const ethersProvider = new ethers.providers.Web3Provider(
    connectedWallet.provider,
    'any'
  )
  console.log(ethersProvider)
}

const onClickConnect = () => {
  const { provider, label } = connectedWallet.value || {}
  if (provider && label) {
    disconnectWallet({ label })
  } else {
    connectWallet()
  }
}

function formatAddress(address) {
  if (!address)
    return

  return `${address.substring(0, 5)}....${address.slice(-4)}`
}

watch(() => connectedWallet.value, () => {



  console.log(
    'connectedWallet watcher\n',
    'walletAddress: ',
    formatAddress(connectedWallet.value?.accounts[0]?.address), '\n',
    'chainId: ',
    connectedWallet.value?.chains[0]?.id
  )
}, { immediate: true })
</script>

<template>
  <main>
    <button @click="onClickConnect">
      {{
        connectingWallet
          ? 'Connecting...'
          : connectedWallet
          ? 'Disconnect'
          : 'Connect'
      }}
    </button>
  </main>
</template>

<style>
main {
  display: flex;
  align-items: center;
  justify-content: center;
}

button {
  border: none;
  border-radius: 6px;
  background: hsla(160, 100%, 37%, 1);
  color: white;
  padding: 14px 12px;
  margin-top: 40px;
  font-family: inherit;
  font-weight: 600;
  font-size: 16px;
  cursor: pointer;
}
</style>
