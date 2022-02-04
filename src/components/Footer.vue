<script setup lang="ts">
import { isDark, toggleDark } from '~/composables'

const { t, availableLocales, locale } = useI18n()

const toggleLocales = () => {
  // change to some real logic
  const locales = availableLocales
  locale.value = locales[(locales.indexOf(locale.value) + 1) % locales.length]
}
</script>

<template>
  <nav class="text-xl mt-6">
    <router-link class="icon-btn mx-2" to="/" :title="t('button.home')">
      <carbon-campsite />
    </router-link>

    <button class="icon-btn mx-2 !outline-none" :title="t('button.toggle_dark')" @click="toggleDark()">
      <carbon-moon v-if="isDark" />
      <carbon-sun v-else />
    </button>

    <a class="hidden icon-btn mx-2" :title="t('button.toggle_langs')" @click="toggleLocales">
      <carbon-language />
    </a>

    <router-link class=" icon-btn mx-2" to="/nftgallery" :title="t('button.nft')">
      <carbon-image />
    </router-link>
    <router-link class="icon-btn mx-2" to="/information" :title="t('button.information')">
      <carbon-dicom-overlay />
    </router-link>
    <router-link class=" hidden icon-btn mx-2" to="/about" :title="t('button.about')">
      <carbon-dicom-overlay />
    </router-link>

    <a class="icon-btn mx-2" rel="noreferrer" href="https://github.com/kodevasse" target="_blank" title="GitHub">
      <carbon-logo-github />
    </a>
    <a class="icon-btn mx-2" rel="noreferrer" href="https://twitter.com/PDPkrypto" target="_blank" title="GitHub">
      <carbon-logo-twitter />
    </a>
    <a class="icon-btn mx-2" rel="noreferrer" href="https://instagram.com/PDPkrypto" target="_blank" title="GitHub">
      <carbon-logo-instagram />
    </a>
  </nav>
</template>
