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
const value = ref([]);
</script>

<template>
  <div
    class="flex min-h-screen flex-col items-center justify-center gap-4 bg-gradient-to-b from-yellow-100 to-red-100"
  >
    <h1
      class="bg-gradient-to-r from-red-300 to-yellow-300 bg-clip-text text-5xl font-extrabold text-transparent"
    >
      Vue Select
    </h1>
    <VueSelect :options="options" v-model="value" :multiple="true" />
  </div>
</template>
