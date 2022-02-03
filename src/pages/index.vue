<script setup lang="ts">
import { storeToRefs } from 'pinia'
import { useCryptoStore } from '~/stores/crypto'
const messageInput = ref(null as any)
const cryptoStore = useCryptoStore()
const { wave, connectWallet } = useCryptoStore()
const { account, guestPosts, guestPostsCount } = storeToRefs(cryptoStore)
</script>

<template>
  <div class="flex flex-col items-center">
    <h1 class="text-2xl m-4">
      PDPkrypto's Guest Book
    </h1>
    <div class="flex flex-col">
      <p class="text-sm mb-3 border-b-3 font-mono">
        <span class="mb-2">Sign up to my guestbook @ <span class="text-pink-400 font-bold">Polygon testnet Mumbai</span>
        </span>
      </p>
      <p class="text-sm font-thin mb-3">
        <span>Get some free Polygon (Mumbai) testnet tokens <a class="hover:bg-pink-400 hover:text-teal-100 bg-teal-300 rounded  px-2 font-bold text-teal-800" href="https://faucet.polygon.technology/" target="_blank">here
        </a>
        </span>
      </p><p class="text-sm mb-3 font-thin">
        Automatically add custom Polygon Mumbai to Metamask <a class=" hover:bg-pink-400 hover:text-teal-100 bg-teal-300 rounded  px-2 font-bold text-teal-800" href="https://chainlist.org/" target="_blank">network (search for Mumbai)

        </a>
      </p>
    </div>
    <button v-if="!account" class="hover:bg-teal-300 bg-green-400 rounded p-4" @click="connectWallet">
      Connect Wallet
    </button>
    <div v-if="account" class="mt-5">
      <input
        v-model="messageInput"
        name="guestBookInfo"
        class="py-4 px-4  shadow border rounded text-gray-500"
        maxlength="20"
      >
      <button class="bg-yellow-300 rounded p-4 mt-10" @click="wave(messageInput)">
        Send
      </button>
    </div>

    <div v-if="account" class="border shadow  sm:w-4/12  p-4 mt-10">
      <h3 class="text-2xl">
        Number Of Entries: {{ guestPostsCount }}
      </h3>
      <div v-for="(guestPost, idx) in guestPosts" :key="idx" class="flex flex-col  m-auto " :class="{'mt-4': idx > 1}">
        <div v-if="guestPost.message" class="flex justify-between w-full">
          <span class="font-semibold">{{ guestPost.timestamp }}</span>
          <span>{{ guestPost.message }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<route lang="yaml">
meta:
  layout: home
</route>
