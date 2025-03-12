<template>
  <div class="container">
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/micasino.png" class="logo vue" alt="Vue logo" />
    </a>
    <FormComponent :fieldDefinitions="fieldDefinitions" />
  </div>
</template>

<script setup lang="ts">
import FormComponent from './components/FormComponent.vue';

const fieldDefinitions = [
  {
    id: 'name',
    label: 'Nombre',
    type: 'Text',
    required: true,
  },
  {
    id: 'options',
    label: 'Options',
    type: 'SelectUnselect',
    options: [
      {
        id: '1',
        label: 'Option 1',
      },
      {
        id: '2',
        label: 'Option 2',
      },
      {
        id: '3',
        label: 'Option 3',
      },
    ],
    required: true,
  }
];
</script>

<style scoped>

.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #e3bd00aa);
}
</style>
