<script setup>
import EntradaSalida from './components/EntradaSalida.vue';
import Dropdown from './components/Dropdown.vue';
import { computed, ref } from 'vue';
import DropdownDias from './components/DropdownDias.vue';
import PhoneNumber from './components/PhoneNumber.vue';
import NameField from './components/NameField.vue';
const fecha = ref(new Date());
const anioHoy = computed(()=>{
  return fecha.value.getFullYear();
});
const mesHoy = computed(()=>{
  return fecha.value.getMonth() + 1;
});
const diaHoy = computed(()=>{
  return fecha.value.getDate();
});

function onChangeDay(day) {
  console.log(`Día seleccionado: ${day}`);
}



function onChangeAnio(anio) {
  anioHoy.value= new Date(anio).getFullYear();
  console.log(`Año seleccionado: ${anio}`);
}

function onDateMonthChanged(year, month) {
  fecha.value = new Date(year, month - 1);
  console.log(`fecha ${fecha.value}`);
  console.log(`Mes seleccionado: ${month} del año ${year}`);
}

function onFullYear(year) {
  console.log(`Año completo seleccionado: ${year}`);
}

</script>
<template>
    <EntradaSalida @on-toggle="onChange"/>
    <Dropdown :year="anioHoy" :month="mesHoy" @onDateMonthChanged="onDateMonthChanged" @onFullYear="onFullYear" @onChangeAnio="onChangeAnio" />
    <Dropdown :year="anioHoy" :month="mesHoy" @onDateMonthChanged="onDateMonthChanged" @onFullYear="onFullYear"  @onChangeAnio="onChangeAnio"/>
    <div/>
    <h2>Dropdown Dias</h2><br/>
    <DropdownDias :day="diaHoy" :year="anioHoy" :month="mesHoy"  @onFullYear="onFullYear" @onChangeAnio="onChangeAnio" @onDateMonthChanged="onDateMonthChanged" @onChangeDay="onChangeDay"/>
    <DropdownDias :day="diaHoy" :year="anioHoy" :month="mesHoy"  @onFullYear="onFullYear" @onChangeAnio="onChangeAnio" @onDateMonthChanged="onDateMonthChanged" @onChangeDay="onChangeDay"/>
    <PhoneNumber initialNumber="9515108874"/>
    <NameField/>
    </template>
<style scoped></style>
