<template>
  <q-page class="page">
    <example-component
      title="Example component"
      active
      class="title row items-top justify-center"
    ></example-component>
    <div class="flex-container">
      <div>
        <QCard class="card">
          <RouterLink to="/feed">
            <q-icon name="dynamic_feed" size="200px"/>
          </RouterLink>
        </QCard>
        <p class="card-label">Feed</p>
      </div>
      <div>
        <QCard class="card">
          <RouterLink to="/cardapio">
            <q-icon name="restaurant_menu" size="200px"/>
          </RouterLink>
        </QCard>
        <p class="card-label">Cardápio</p>
      </div>
      <div>
        <QCard class="card">
          <RouterLink to="/publicar">
            <q-icon name="send" size="200px"/>
          </RouterLink>
        </QCard>
        <p class="card-label">Publicar</p>
      </div>
      <div>
        <QCard class="card">
          <RouterLink to="/calendario">
            <q-icon name="event" size="200px"/>
          </RouterLink>
        </QCard>
        <p class="card-label">Calendário</p>
      </div>
      <div>
        <QCard class="card">
          <RouterLink to="/disciplinas">
            <q-icon name="school" size="200px"/>
          </RouterLink>
        </QCard>
        <p class="card-label">Disciplinas</p>
      </div>
      <div>
        <QCard class="card">
          <!-- Ajuste aqui para apontar para a nova rota -->
          <RouterLink to="/app/configuracoes">
            <q-icon name="settings" size="200px"/>
          </RouterLink>
        </QCard>
        <p class="card-label">Configurações</p>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
</script>

<style scoped>
  .page {
    background-color: whitesmoke;
  }
  .title {
    display: flex;
  }

  .flex-container {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-around;
    align-items: center;
    align-content: space-around;
  }

  .card {
    display: flex;
    background: linear-gradient(0deg, #31813e 0%, #4e53ef 100%);
    font-style: italic;
    color: #ffffff;
    justify-content: center;
    min-width: 400px;
    min-height: 200px;
    cursor: pointer;
    margin: 20px;
    box-shadow: 5px 5px 10px 1px #4e53ef;
    border-radius: 10px;
  }
  .card-label {
    display: flex;
    justify-content: center;
    font-size: 15px;
    font-weight: bold;
  }
</style>