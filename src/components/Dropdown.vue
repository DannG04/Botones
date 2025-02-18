<template>
  <div class="dropdown">
    <button class="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton" data-bs-toggle="dropdown"
      aria-expanded="false">
      <iconify-icon icon="mdi:calendar" :width="15" inline />
        {{ selectedLabel }}
    </button>
    <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton">
      <li class="dropdown-item">
        <div class="dropdown">
          <button class ="butonVerde" @click.stop.prevent="changeYear(-1)"><iconify-icon icon="ic:outline-arrow-left"></iconify-icon></button>
          <span class="year-text">{{ currentYear }}</span>
          <button class= "butonVerde" @click.stop.prevent="changeYear(1)"><iconify-icon icon="ic:outline-arrow-right"></iconify-icon></button>
        </div>
      </li>
      <li class="dropdown-item">
        <div class="grid-container">
          <div v-for="(month, index) in months" :key="index" class="grid-item">
            <a  @click.prevent="selectMonth(index)" class="month-cell">{{ month }}</a>
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
  closeDropdown();
}

function selectFullYear() {
  selectedLabel.value = `Todo ${currentYear.value}`;
  emit('onFullYear', currentYear.value);
  closeDropdown();
}

function closeDropdown() {
  const dropdownMenuButton = document.getElementById('dropdownMenuButton');
  if (dropdownMenuButton) {
    dropdownMenuButton.setAttribute('aria-expanded', 'false');
    dropdownMenuButton.classList.remove('show');
    const dropdownMenu = dropdownMenuButton.nextElementSibling;
    if (dropdownMenu) {
      dropdownMenu.classList.remove('show');
    }
  }
}
</script>

<style scoped>
.dropdown-menu {
  max-height: 300px;
  background-color: #16a34a;
  color: white;
}

.butonVerde{
  background-color: #16a34a;
  color: white;
  border-radius: 0;
  border-color: #16a34a;
  margin-left: 5px;
  margin-right: 5px;
}



.dropdown-item {
  background-color: #16a34a;
  color: white;
  border-radius: 0;
}

.dropdown div {
  background-color: #16a34a;
  max-width: 130px;
  color: white;
  border-radius: 0;
  text-align: center;
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(3, 40px);
  gap: 1px;
}

.grid-item {
  text-align: center;
  padding: 0%;

}

.month-cell {
  color: white;
  background-color: #16a34a; 
  display: block;
}
.month-cell:hover{
  background-color: #0e5e2c; 
  cursor: pointer;
}



.full-year {
  display: block;
  width: 100%;
  height: 100%;
  text-align: center;
  background-color: #16a34a;
  color: white;
  border-radius: 0;
}
.full-year:hover {
  background-color: #0e5e2c; 
}

.year-text {
  color: white;
}
</style>
