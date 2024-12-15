<template>
  <div class="dropdown">
    <button class="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton" data-bs-toggle="dropdown"
      aria-expanded="false">
      {{ selectedLabel }}
    </button>
    <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton">
      <li class="dropdown-item">
        <div class="d-flex justify-content-between align-items-center">
          <a href="#" @click.prevent="changeYear(-1)"><iconify-icon icon="ic:outline-arrow-left"></iconify-icon></a>
          <span class="year-text">{{ currentYear }}</span>
          <a href="#" @click.prevent="changeYear(1)"><iconify-icon icon="ic:outline-arrow-right"></iconify-icon></a>
        </div>
      </li>
      <li class="dropdown-item">
        <div class="grid-container">
          <div v-for="(month, index) in months" :key="index" class="grid-item">
            <a href="#" @click.prevent="selectMonth(index)" class="d-block text-center month-cell">{{ month }}</a>
          </div>
        </div>
      </li>
      <li class="dropdown-item text-center">
        <a href="#" @click.prevent="selectFullYear" class="full-year">{{ `Todo ${currentYear}` }}</a>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import '@iconify/iconify';

const currentYear = ref(new Date().getFullYear());
const selectedLabel = ref('Seleccionar Mes/Año');
const months = ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun', 'Jul', 'Ago', 'Sept', 'Oct', 'Nov', 'Dic'];
const emit = defineEmits(['onDateMonthChanged', 'onFullYear']);

function changeYear(direction) {
  currentYear.value += direction;
}

function selectMonth(monthIndex) {
  selectedLabel.value = `${months[monthIndex]} ${currentYear.value}`;
  emit('onDateMonthChanged', currentYear.value, monthIndex + 1);
}

function selectFullYear() {
  selectedLabel.value = `Todo ${currentYear.value}`;
  emit('onFullYear', currentYear.value);
}
</script>

<style scoped>
.dropdown-menu {
  max-height: 300px;
  overflow-y: auto;
  background-color: #343a40;
  color: white;
  border-radius: 0; /* Remove rounded corners */
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
}

.grid-item {
  text-align: center;
}

.month-cell {
  padding: 1px 0;
  margin: 0;
  color: white;
  border-radius: 0; /* Remove rounded corners */
}

.full-year {
  display: block;
  width: 100%;
  text-align: center;
  color: white;
  border-radius: 0; /* Remove rounded corners */
}

.year-text {
  color: white;
}
</style>
