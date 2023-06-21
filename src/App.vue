<script setup lang="ts">
// import { RouterLink, RouterView } from 'vue-router'
import { RouterView } from 'vue-router'
import { titlePage  } from '@services/helpers'

titlePage(/* defaul */)
</script>

<template>
  <!-- <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" /> -->
  <!-- <HelloWorld msg="You did it!" /> -->
  <!-- <RouterLink to="/">Home</RouterLink> -->

  <RouterView />
</template>

<style scoped>
</style>
