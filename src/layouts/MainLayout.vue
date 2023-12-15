<template>
  <q-layout view="hHh lpR ffr">
  <q-header elevated class="bg-primary text-white">
      <q-toolbar>
          <q-toolbar-title>
              <q-avatar>
                  <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg">
              </q-avatar>
              Title
          </q-toolbar-title>
          <q-btn dense flat round icon="menu" @click="toggleRightDrawer" />
      </q-toolbar>
  </q-header>
  <q-drawer show-if-above v-model="rightDrawerOpen" side="right" bordered>
      <!-- drawer content -->
  </q-drawer>
      <q-page-container>
          <router-view />
      </q-page-container>
  </q-layout>
</template>
<script lang="ts" setup>
import { ref } from 'vue';


const rightDrawerOpen = ref(false)

const toggleRightDrawer = () => {
  rightDrawerOpen.value = !rightDrawerOpen.value
}

</script>
<style scoped>

</style>
