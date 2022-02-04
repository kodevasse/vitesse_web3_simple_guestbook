<script setup lang="ts">
import { ethers } from 'ethers'
import { acceptHMRUpdate, storeToRefs } from 'pinia'
import { useCryptoStore } from '~/stores/crypto'
const messageInput = ref(null as any)
const cryptoStore = useCryptoStore()
const { wave, connectWallet } = useCryptoStore()
const { account, guestPosts, guestPostsCount } = storeToRefs(cryptoStore)
const transactions = ref()
const res = ref()

async function getOpenseaItems() {
  const items = await fetch('https://api.etherscan.io/api?module=account&action=txlist&address=0xA14A2A78A9583CD6C1CB926461B6ED4571201140&startblock=0&endblock=99999999&page=1&offset=10&sort=asc&apikey=FH8UXWMNTS3S2I45Z1MGWQZ5YU8Y9HE5NF')
    .then(res => res.json())
    .then((res) => {
      return res.result
    })
    .catch((e) => {
      console.error(e)
      console.error('Could not talk to etherscan')
      return null
    })
}
const formatDate = (date: Date) =>
  `${date.getHours()}:${String(date.getMinutes()).padStart(2, '0')} ${String(
    date.getSeconds(),
  ).padStart(2, '0')}.${String(date.getMilliseconds()).padStart(3, '0')}`

interface PokemonData {
  id: string
  number: string
  name: string
  image: string
  fetchedAt: string
  attacks: {
    special: Array<{
      name: string
      type: string
      damage: number
    }>
  }
}

async function fetchPokemon(name: string): Promise<PokemonData> {
  const pokemonQuery = `
    query PokemonInfo($name: String) {
      pokemon(name: $name) {
        id
        number
        name
        image
        attacks {
          special {
            name
            type
            damage
          }
        }
      }
    }
  `

  const response = await window.fetch('https://graphql-pokemon2.vercel.app/', {
    // learn more about this API here: https://graphql-pokemon2.vercel.app/
    method: 'POST',
    headers: {
      'content-type': 'application/json;charset=UTF-8',
    },
    body: JSON.stringify({
      query: pokemonQuery,
      variables: { name: name.toLowerCase() },
    }),
  })

  interface JSONResponse {
    data?: {
      pokemon: Omit<PokemonData, 'fetchedAt'>
    }
    errors?: Array<{ message: string }>
  }
  const { data, errors }: JSONResponse = await response.json()
  if (response.ok) {
    const pokemon = data?.pokemon
    if (pokemon) {
      // add fetchedAt helper (used in the UI to help differentiate requests)
      return Object.assign(pokemon, { fetchedAt: formatDate(new Date()) })
    }
    else {
      return Promise.reject(new Error(`No pokemon with the name "${name}"`))
    }
  }
  else {
    // handle the graphql errors
    const error = new Error(errors?.map(e => e.message).join('\n') ?? 'unknown')
    return Promise.reject(error)
  }
}
async function pikachuIChooseYou() {
  const pikachu = await fetchPokemon('pikachu')
  console.log(pikachu.attacks.special)
}
</script>

<template>
  <button class="rounded bg-blue-500 hover:bg-blue-700 py-2 px-4 text-white" @click="pikachuIChooseYou()">
    Get OpenSea Assets
  </button>

  <div class="w-full flex justify-center">
    <div v-for="item in res" :key="item" class="w-1/2 grid grid-cols-4 gap-2">
      {{ item }}
    </div>
  </div>
</template>

<route lang="yaml">
meta:
  layout: home
</route>
