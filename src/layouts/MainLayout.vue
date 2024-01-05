<template>
  <q-layout view="hHh lpR ffr">
  <q-header elevated class="bg-primary text-white">
      <q-toolbar>
          <q-toolbar-title>
              <q-avatar>
                  <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg">
              </q-avatar>
              Unb na Mão
          </q-toolbar-title>
          <q-btn dense flat round icon="menu" @click="toggleRightDrawer" />
      </q-toolbar>
  </q-header>
  <q-drawer show-if-above v-model="rightDrawerOpen" side="right" bordered>
      <q-img class="absolute-top bg-blue"
        style="height: 140px"
      >
        <div class="row items-center justify-center bg-transparent">
          <q-avatar size="80px" class="q-mb-sm">
            <img style="border-bottom: 2px solid rgb(202, 202, 202)" >
          </q-avatar>
          <div class="column q-pl-md">
            <div class="session-nome text-weight-bold w100">{{ }}</div>
            <div class="low-opacity w100" style="font-size: 12px;">{{  }}</div>
          </div>
          <div class="low-opacity w100 text-center">@{{ }}</div>
        </div>
      </q-img>
    <q-scroll-area style="height: calc(100% - 140px); margin-top: 140px; border-right: 1px solid #ddd">
      <q-list padding>
        <q-item clickable v-ripple>
          <q-item-section avatar>
            <q-icon name="admin_panel_settings" color="orange-10" />
          </q-item-section>

          <q-item-section style="color: rgb(255, 154, 39); font-weight: bold;">
            Admin Logs
          </q-item-section>
        </q-item>

        <q-item clickable v-ripple to="/feed">
          <q-item-section avatar>
            <q-icon name="dynamic_feed" />
          </q-item-section>

          <q-item-section>
            Feed
          </q-item-section>
        </q-item>

        <q-item clickable v-ripple to="/cardapio">
          <q-item-section avatar>
            <q-icon name="restaurant_menu" />
          </q-item-section>

          <q-item-section>
            Cardápio
          </q-item-section>
        </q-item>

        <q-item clickable v-ripple to="/publicar">
          <q-item-section avatar>
            <q-icon name="send" />
          </q-item-section>

          <q-item-section>
            Publicar
          </q-item-section>
        </q-item>
          <q-item clickable v-ripple to="/calendario">
          <q-item-section avatar>
            <q-icon name="event" />
          </q-item-section>

          <q-item-section>
            Calendário
          </q-item-section>
        </q-item>
        <q-item clickable v-ripple to="/disciplinas">
          <q-item-section avatar>
            <q-icon name="school" />
          </q-item-section>

          <q-item-section>
            Disciplinas
          </q-item-section>
        </q-item>
        <q-item clickable v-ripple  to="/configuracoes">
          <q-item-section avatar>
            <q-icon name="settings" />
          </q-item-section>

          <q-item-section>
            Configurações
          </q-item-section>
        </q-item>

        <q-item clickable v-ripple @click="" class="logout-section">
          <q-item-section avatar>
            <q-icon name="logout" />
          </q-item-section>

          <q-item-section>
            Fazer Logout
          </q-item-section>
        </q-item>
        <meta name="google-adsense-account" content="ca-pub-9172295644756939">
        <q-item clickable v-ripple class="absolute-bottom q-mb-sm" @click="">
          <q-item-section avatar>
            <q-icon name="git" />
          </q-item-section>

          <q-item-section class="low-opacity">
            UnB na Mão [ Antares ]
          </q-item-section>
        </q-item>
      </q-list>
    </q-scroll-area>
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
  .card{
      font-size: x-large;
      display: flex;
      font-style: italic;
      border: 2px;
      border-radius: 5px;
      box-shadow: 0px 0px 5px 5px #3f8bee;
      min-width: 250px;
      justify-content: center;
      cursor: pointer;
      margin: 5px;
    }
  .logout-section{
      margin-bottom: 50px;
    }
</style>
