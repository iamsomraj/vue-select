<script setup>
import { ref } from "vue";
import VueSelect from "./components/VueSelect.vue";

const options = [
  { label: "First", value: 1 },
  { label: "Second", value: 2 },
  { label: "Third", value: 3 },
  { label: "Fourth", value: 4 },
  { label: "Fifth", value: 5 },
];
const value = ref(options[0]);
</script>

<template>
  <div
    class="flex min-h-screen items-center justify-center bg-gradient-to-b from-yellow-100 to-red-100"
  >
    <VueSelect :options="options" v-model="value" />
  </div>
</template>
