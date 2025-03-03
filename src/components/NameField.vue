<template>
  <div>
    <input 
      type="text" 
      id="name" 
      v-model="name" 
      @input="formatName" 
      placeholder="Enter your name" 
      maxlength="15"
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: ''
    };
  },
  methods: {
    formatName() {
      // Remove numbers, ensure only the first letter is capitalized, and limit length to 15
      this.name = this.name.replace(/[^a-zA-Z\s]/g, '').toLowerCase().replace(/\b\w/, char => char.toUpperCase()).substring(0, 15);
    }
  }
};
</script>

<style scoped>
/* Add any styles you need here */
</style>
