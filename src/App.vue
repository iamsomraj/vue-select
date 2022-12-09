<script setup>
import VueSelect from "./components/VueSelect.vue";
const options = [
  { label: "First", value: 1 },
  { label: "Second", value: 2 },
  { label: "Third", value: 3 },
  { label: "Fourth", value: 4 },
  { label: "Fifth", value: 5 },
];
const value = ref(1);
</script>

<template>
  <VueSelect :options="options" v-model="value" />
</template>
