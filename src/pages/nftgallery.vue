<script setup lang="ts">
import { ethers } from 'ethers'
import { acceptHMRUpdate, storeToRefs } from 'pinia'
import { useCryptoStore } from '~/stores/crypto'
const messageInput = ref(null as any)
const cryptoStore = useCryptoStore()
const { wave, connectWallet } = useCryptoStore()
const { account, guestPosts, guestPostsCount } = storeToRefs(cryptoStore)

// async function getOpenseaItems() {
//     setLoader(true)
//     try {
//       const { ethereum } = window
//       if (ethereum) {
//         const provider = new ethers.providers.Web3Provider(ethereum)
//         const signer = provider.getSigner()
//         const wavePortalContract = new ethers.Contract(contractAddress, contractABI.abi, signer)
//         const count = (await wavePortalContract.getBalance())
//         const amt = ethers.utils.formatEther(count)
//         console.log('count', amt)
//         setLoader(false)
//       }
//     }
//     catch (e) {
//       setLoader(false)
//     }
//   }
async function getOpenseaItems() {
  const { ethereum } = window
  if (ethereum) {
    const osContainer = document.getElementById('openseaItems')

    const items = await fetch(`https://api.opensea.io/api/v1/assets?owner=${0xA14A2A78A9583CD6C1CB926461B6ED4571201140}&order_direction=desc&offset=0&limit=50`)
      .then(res => res.json())
      .then((res) => {
        return res.assets
      })
      .catch((e) => {
        console.error(e)
        console.error('Could not talk to OpenSea')
        return null
      })

    if (items.length === 0) return

    items.forEach((nft) => {
      const { name, image_url, description, permalink } = nft

      const newElement = document.createElement('div')
      newElement.innerHTML = `
          <!-- Opensea listing item-->
          <a href='${permalink}' target="_blank">
            <div class='flex flex-col'>
              <img
                src='${image_url}'
                class='w-full rounded-lg' />
              <div class='flex-col w-full space-y-1'>
                <p class='text-gray-800 text-lg'>${name}</p>
                <p class='text-gray-500 text-xs word-wrap'>${description ?? ''}</p>
              </div>
            </div>
          </a>
          <!-- End Opensea listing item-->
        `

      osContainer.appendChild(newElement)
    })
  }
}
</script>

<template>
  <button class="rounded bg-blue-500 hover:bg-blue-700 py-2 px-4 text-white" @click="getOpenseaItems()">
    Get OpenSea Assets
  </button>

  <div class="w-full flex justify-center">
    <div id="openseaItems" class="w-1/2 grid grid-cols-4 gap-2">
    <!-- Opensea items go here -->
    </div>
  </div>
</template>

<route lang="yaml">
meta:
  layout: home
</route>
