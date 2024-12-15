<script setup>
import EntradaSalida from './components/EntradaSalida.vue';
import Dropdown from './components/Dropdown.vue';

function onChange(state) {
  console.log('statePadre: ', state);
}

function onDateMonthChanged(year, month) {
  console.log(`Mes seleccionado: ${month} del año ${year}`);
}

function onFullYear(year) {
  console.log(`Año completo seleccionado: ${year}`);
}
</script>

<template>
  <EntradaSalida @on-toggle="onChange" />
  <Dropdown @onDateMonthChanged="onDateMonthChanged" @onFullYear="onFullYear" />
</template>

<style scoped></style>
