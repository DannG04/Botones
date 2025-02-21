<script setup>
import { format, getDaysInMonth } from 'date-fns';
import { es } from 'date-fns/locale';
import Iconify from '@iconify/iconify';

import { ref, defineEmits, defineProps, computed } from 'vue';

const props = defineProps({
  year: {
    type: Number,
    default: new Date().getFullYear()
  },
  month: {
    type: Number,
    default: new Date().getMonth() + 1
  },
  day: {
    type: Number,
    default: new Date().getDate()
  }
});

const currentYear = ref(props.year);
const currentMonth = ref(props.month);
const selectedDay = ref(props.day);
const months = Array.from({ length: 12 }, (_, i) => format(new Date(0, i), 'MMM', { locale: es }));
const selectedLabel = ref(`${selectedDay.value} ${months[currentMonth.value - 1]} ${currentYear.value}`);
const emit = defineEmits(['onDateMonthChanged', 'onFullYear', 'onChangeAnio', 'onChangeDay']);

const daysNumber = computed(() => {
  const daysInMonth = getDaysInMonth(new Date(currentYear.value, currentMonth.value - 1));
  return Array.from({ length: daysInMonth }, (_, i) => format(new Date(currentYear.value, currentMonth.value - 1, i + 1), 'd', { locale: es }));
});

function changeYear(direction) {
  currentYear.value += direction;
  emit('onChangeAnio', currentYear.value);
  updateSelectedLabel();
}

function changeMonth(direction) {
  currentMonth.value += direction;
  if (currentMonth.value < 1) {
    currentMonth.value = 12;
    changeYear(-1);
  } else if (currentMonth.value > 12) {
    currentMonth.value = 1;
    changeYear(1);
  }
  emit('onDateMonthChanged', currentYear.value, currentMonth.value);
  updateSelectedLabel();
}

function selectDay(day) {
    selectedDay.value = day;
    selectedLabel.value = `${day} ${months[currentMonth.value - 1]} ${currentYear.value}`;
    emit('onChangeDay', day);
    closeDropdown();
}

function selectFullYear() {
  selectedLabel.value = `Todo ${currentYear.value}`;
  emit('onFullYear', currentYear.value);
  closeDropdown();
}

function updateSelectedLabel() {
  selectedLabel.value = `${selectedDay.value} ${months[currentMonth.value - 1]} ${currentYear.value}`;
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

<template>
  <div class="dropdown">
    <button class="btn buttonW btn-secondary dropdown-toggle text-capitalize" type="button" id="dropdownMenuButton" data-bs-toggle="dropdown"
      aria-expanded="false">
      <iconify-icon icon="mdi:calendar" :width="15" inline />
      {{ selectedLabel }}
    </button>
    <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton">
      <li class="dropdown-item">
        <div class="dropdown">
          <button class="butonVerde" @click.stop.prevent="changeYear(-1)"><iconify-icon icon="ic:outline-arrow-left"></iconify-icon></button>
          <span class="year-text">{{ currentYear }}</span>
          <button class="butonVerde" @click.stop.prevent="changeYear(1)"><iconify-icon icon="ic:outline-arrow-right"></iconify-icon></button>
          <button class="butonVerde" @click.stop.prevent="changeMonth(-1)"><iconify-icon icon="ic:outline-arrow-left"></iconify-icon></button>
          <span class="month-text text-capitalize">{{ months[currentMonth - 1] }}</span>
          <button class="butonVerde" @click.stop.prevent="changeMonth(1)"><iconify-icon icon="ic:outline-arrow-right"></iconify-icon></button>
        </div>
      </li>
      <li class="dropdown-item">
        <div class="grid-container">
          <div v-for="day in daysNumber" :key="day" class="grid-item">
            <a @click.prevent="selectDay(day)" class="day-cell d-flex align-items-center justify-content-center">{{ day }}</a>
          </div>
        </div>
      </li>
      <li class="dropdown-item">
        <a href="#" @click.prevent="selectFullYear" class="full-year">{{ `Todo ${currentYear}` }}</a>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.dropdown-menu {
  max-height: 320px;
  background-color: #16a34a;
  color: white;
  padding: 10px;
}

.butonVerde {
  background-color: #16a34a;
  color: white;
  border-radius: 0;
  border-color: #16a34a;
  margin-right: 5px;
  padding-bottom: 0px;

}

.dropdown-item {
  background-color: #16a34a;
  color: white;
  padding: 0; 
}

.dropdown div {
  background-color: #16a34a;
  max-width: 300px;
  color: white;
  border-radius: 0;
  text-align: center;
  padding-top: 0px;
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(7, 1fr); 
  gap: 0; 
  padding: 5px;
  padding-bottom: 0px; 
}

.grid-item {
  width: 35px;
  height: 35px;
  padding: 0px;
  margin: 0px;
  gap: 0;
  
}

.day-cell {
  color: white;
  background-color: #16a34a;
  display: block;
  align-items: center;
  justify-content: center;
  border-radius: 5px; 
  font-size: 14px;
  height: 35px;
  padding: 0px; 
  margin: 0px; 
}

.day-cell:hover {
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
  border-radius: 5px; 
  padding: 0px; 
}

.full-year:hover {
  background-color: #0e5e2c;
}

.year-text {
  font-size: 14px;
  padding: 10px;
}
.month-text {
  font-size: 14px;
  padding-right: 10px;
  padding-left: 10px;
  width: 50px; 
  display: inline-block;
  text-align: center;
}

.buttonW{
  width: 150px; 
  display: inline-block;
  text-align: center;
}
</style>
