<script setup>
import { ref } from 'vue';
const emit = defineEmits(["onToggle"]);
const state = ref(true);
const label = ref("Entrada")
function onChangeStates(flag) {
  state.value = !flag
  console.log('state.value: ', state.value);
  label.value = state.value ? "Entrada" : "Salida";
  emit("onToggle", state.value);
}
</script>

<template>
  <div>
    <button type="button" class="btn btn-primary dark-theme" data-bs-toggle="button" autocomplete="off"
      @click.prevent="onChangeStates(state)">
      {{ label }}
      <iconify-icon :icon="state ? 'fluent:arrow-step-out-20-filled' : 'fluent:arrow-step-in-20-filled'" />
    </button>
  </div>
</template>

<style scoped>
.dark-theme {
  background-color: #333;
  color: #fff;
}

.dark-theme.btn-primary {
  background-color: #555;
  border-color: #444;
}

.dark-theme.btn-primary:hover {
  background-color: #0c0808;
  border-color: #555;
}
</style>
