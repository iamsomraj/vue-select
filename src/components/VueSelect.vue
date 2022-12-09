<script setup>
/* IMPORTS */
import VueSingleSelect from "./VueSingleSelect.vue";
import VueMultiSelect from "./VueMultiSelect.vue";
import { useAttrs } from "vue";
/* IMPORTS */

/* PROPS */
const props = defineProps({ multiple: { type: Boolean, default: false } });
/* PROPS */

/* ATTRIBUTES */
const attrs = useAttrs();
/* ATTRIBUTES */
</script>

<template>
  <VueSingleSelect v-if="!props.multiple" v-bind="attrs" />
  <VueMultiSelect v-else v-bind="attrs" />
</template>
