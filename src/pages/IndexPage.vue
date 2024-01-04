<template>
  <q-page class="page">
    <example-component
      title="Example component"
      active
      class="title row items-top justify-center"
    ></example-component>
    <div class="row items-center justify-evenly">
      <QCard class="card">
        <q-icon name="dynamic_feed" size="100px"/>
      </QCard>
      <QCard class="card">
        <q-icon name="restaurant_menu" size="100px"/>
      </QCard>
      <QCard class="card">
        <q-icon name="send" size="100px"/>
      </QCard>
      <QCard class="card">
        <q-icon name="event" size="100px"/>
      </QCard>
      <QCard class="card">
        <q-icon name="school" size="100px"/>
      </QCard>
      <QCard class="card">
        <q-icon name="settings" size="100px"/>
      </QCard>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { Todo, Meta } from 'components/models';
import ExampleComponent from 'components/ExampleComponent.vue';
import { ref } from 'vue';
</script>

<style scoped>
  .page{
    background-color: whitesmoke;
  }
  .title{
    display: flex;
  }

  .card{
    display: flex;
    background: linear-gradient(0deg, #31813e 0%, #4e53ef 100%);
    font-style: italic;
    color: #ffffff;
    justify-content: center;
    justify-items: center;
    min-width: 200px;
    min-height: 100px;
    cursor: pointer;
    margin: 20px;
    box-shadow: 5px 5px 10px 1px #4e53ef;
    border-radius: 10px;
  }
</style>
