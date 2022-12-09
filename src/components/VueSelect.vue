<script setup>
/* IMPORTS */
import { ref, computed } from "vue";
/* IMPORTS */

/* PROPS */
const props = defineProps({
  widthStyles: {
    type: String,
    default: "w-64",
  },
  options: {
    type: Array,
    value: (value) => value.every((option) => option.label && option.value),
  },
  modelValue: {
    type: Object,
    value: (value) => value.label && value.value,
  },
});
/* PROPS */

/* EMITS */
const emits = defineEmits(["update:modelValue", "input", "change"]);
/* EMITS */

const isOpen = ref(false);
const highlightedIndex = ref(0);
const widthStyles = computed(() => props.widthStyles);
const modelValue = computed(() => props.modelValue);

const toggleIsOpen = () => (isOpen.value = !isOpen.value);

const clearOption = (event) => {
  event.stopPropagation();
  emits("update:modelValue", null);
  emits("input", null);
  emits("change", null, modelValue.value);
};

const selectOption = (event, option) => {
  event.stopPropagation();

  if (option.value === modelValue.value) {
    isOpen.value = false;
    return;
  }

  emits("update:modelValue", option);
  emits("input", option);
  emits("change", option, modelValue.value);

  isOpen.value = false;
};
const isOptionSelected = (option) => option.value === modelValue.value.value;
</script>

<template>
  <!-- SELECT CONTAINER -->
  <div
    tabindex="0"
    class="select-container"
    @click="toggleIsOpen"
    @blur="isOpen = false"
    :class="{
      [widthStyles]: true,
    }"
  >
    <!-- VALUE -->
    <span class="value">{{ modelValue?.label }}</span>
    <!-- VALUE -->

    <!-- CLEAR BUTTON -->
    <button class="clear-btn" @click="clearOption">&times;</button>
    <!-- CLEAR BUTTON -->

    <!-- DIVIDER -->
    <div class="divider"></div>
    <!-- DIVIDER -->

    <!-- CARET -->
    <div class="caret">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 20 20"
        fill="currentColor"
      >
        <path
          fill-rule="evenodd"
          d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
          clip-rule="evenodd"
        />
      </svg>
    </div>
    <!-- CARET -->

    <!-- OPTIONS -->
    <ul
      class="options"
      :class="{
        show: isOpen,
      }"
    >
      <!-- OPTION ITEM -->
      <li
        v-for="(option, index) in options"
        :key="option.value"
        class="option"
        :class="{
          selected: isOptionSelected(option),
          highlighted: highlightedIndex === index,
        }"
        @click="selectOption($event, option)"
        @mouseenter="highlightedIndex = index"
      >
        {{ option.label }}
      </li>
      <!-- OPTION ITEM -->
    </ul>
    <!-- OPTIONS -->
  </div>
  <!-- SELECT CONTAINER -->
</template>

<style lang="postcss" scoped>
.select-container {
  @apply relative flex items-center gap-2 rounded-md border border-gray-300 bg-white p-2 outline-none;
}
.select-container:focus {
  @apply border-blue-500;
}
.value {
  @apply flex-grow;
}
.clear-btn {
  @apply cursor-pointer p-0 text-lg text-gray-500 outline-none;
}
.clear-btn:hover,
.clear-btn:focus {
  @apply text-gray-700;
}

.divider {
  @apply w-px self-stretch bg-gray-300;
}
.caret {
  @apply h-5 w-5 text-gray-500;
}
.options {
  @apply absolute left-0 top-full my-2 hidden max-h-60 w-full overflow-y-auto rounded-md border border-gray-300 bg-white p-0;
}

.options.show {
  @apply block;
}

.option {
  @apply cursor-pointer p-2 text-gray-700;
}

.option.selected {
  @apply bg-blue-100;
}
.option.highlighted {
  @apply bg-blue-500 text-white;
}
</style>
