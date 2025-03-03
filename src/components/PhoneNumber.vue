<script setup>
import { ref, computed, watch } from 'vue';

const emit = defineEmits(['update:formattedNumber']);

const rawValue = ref('');
const isValid = ref(true);
const touched = ref(false);

const props = defineProps({
  formattedNumber: {
    type: String,
    default: ''
  },
  initialNumber: {
    type: String,
    default: ''
  }
});

const displayValue = computed({
  get() {
    return formatNumber(rawValue.value);
  },
  set(newValue) {
    rawValue.value = newValue.replace(/\D/g, '').slice(0, 10);
  }
});

const applyFormat = (value) => {
  const part1 = value.slice(0, 3);
  const part2 = value.slice(3, 6);
  const part3 = value.slice(6, 10);
  let formatted = part1;
  if (part2) {
    formatted += `-${part2}`;
  }
  if (part3) {
    formatted += `-${part3}`;
  }
  return formatted;
};

const formatNumber = (value) => {
  const cleaned = value.replace(/\D/g, '').slice(0, 10);
  return applyFormat(cleaned);
};

const handleKeydown = (event) => {
  const allowedKeys = [
    'Backspace', 'Delete', 'Tab', 
    'ArrowLeft', 'ArrowRight', 'ArrowUp', 'ArrowDown'
  ];
  
  if (!event.key.match(/^\d$/) && !allowedKeys.includes(event.key)) {
    event.preventDefault();
  }
};

const handlePaste = (event) => {
  event.preventDefault();
  const pasted = event.clipboardData.getData('text/plain').replace(/\D/g, '');
  const combined = (rawValue.value + pasted).slice(0, 10);
  rawValue.value = combined;
};

const handleInput = (event) => {
  const input = event.target.value.replace(/\D/g, ''); // Eliminar caracteres no numéricos
  rawValue.value = input;
  isValid.value = input.length === 10;
  emit('update:formattedNumber', formatNumber(input));
  touched.value = true;
};

watch(rawValue, (newVal) => {
  isValid.value = newVal.length === 10;
  emit('update:formattedNumber', formatNumber(newVal));
});

// Watch for changes in initialNumber prop
watch(() => props.initialNumber, (newVal) => {
  rawValue.value = newVal.replace(/\D/g, '').slice(0, 10);
}, { immediate: true });

</script>

<template>
  <div class="mb-3">
    <input
      type="tel"
      v-model="displayValue"
      @keydown="handleKeydown"
      @paste="handlePaste"
      @input="handleInput"
      class="form-control"
      :class="{ 'is-invalid': touched && !isValid, 'is-valid': touched && isValid }"
      placeholder="Ej: 123-456-7890"
      maxlength="12"
      inputmode="numeric"
    />
    <div v-if="touched && !isValid" class="invalid-feedback">
      Solo números de 10 dígitos
    </div>
  </div>
</template>
<style scoped>
  input {
    width: 150px;
  }
</style>
