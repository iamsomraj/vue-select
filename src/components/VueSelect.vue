<script setup>
import { ref } from "vue";

/* PROPS */
const props = defineProps({
  options: {
    type: Array,
    required: true,
  },
  value: {
    type: Number,
    required: true,
  },
});
/* PROPS */

/* EMITS */
const emits = defineEmits(["update:modelValue", "input", "change"]);
/* EMITS */
const isOpen = ref(false);
</script>

<template>
  <div>{{ isOpen }}</div>
</template>
