<script setup>
/* IMPORTS */
import { ref, computed, watch } from "vue";
/* IMPORTS */

/* PROPS */
const props = defineProps({
  searchable: {
    type: Boolean,
    default: true,
  },
  widthStyles: {
    type: String,
    default: "w-64",
  },
  options: {
    type: Array,
    value: (value) => value.every((option) => option.label && option.value),
  },
  modelValue: {
    type: Array,
    value: (value) => value.every((option) => option.label && option.value),
  },
});
/* PROPS */

/* EMITS */
const emits = defineEmits(["update:modelValue", "input", "change"]);
/* EMITS */

const isOpen = ref(false);
const highlightedIndex = ref(0);
const searchFilter = ref("");
const widthStyles = computed(() => props.widthStyles);
const modelValue = computed(() => props.modelValue);
const searchable = computed(() => props.searchable);
const options = computed(() => {
  if (!searchable.value) {
    return props.options;
  }
  // Filter options by searchFilter
  return props.options.filter((option) =>
    option.label.toLowerCase().includes(searchFilter.value.toLowerCase())
  );
});

const toggleIsOpen = () => (isOpen.value = !isOpen.value);

const clearOption = (event) => {
  event.stopPropagation();
  emits("update:modelValue", []);
  emits("input", []);
  emits("change", [], modelValue.value);
};

const selectOption = (event, option) => {
  event.stopPropagation();

  /* IF OPTION ALREADY IS SELECTED, THEN CLOSE AND RETURN  */
  if (isOptionSelected(option)) {
    isOpen.value = false;
    return;
  }

  emits("update:modelValue", [...modelValue.value, option]);
  emits("input", [...modelValue.value, option]);
  emits("change", [...modelValue.value, option], [...modelValue.value]);

  isOpen.value = false;

  if (searchable.value && searchFilter?.value?.trim()?.length > 0) {
    searchFilter.value = "";
  }
};

const removeOption = (event, option) => {
  event.stopPropagation();

  if (!isOptionSelected(option)) {
    isOpen.value = false;
    return;
  }

  emits(
    "update:modelValue",
    modelValue.value.filter((item) => item.value !== option.value)
  );
  emits(
    "input",
    modelValue.value.filter((item) => item.value !== option.value)
  );
  emits(
    "change",
    modelValue.value.filter((item) => item.value !== option.value),
    [...modelValue.value]
  );

  isOpen.value = false;
};

const isOptionSelected = (option) => {
  return modelValue.value.some((item) => item.value === option.value);
};

watch(
  () => isOpen.value,
  (value) => {
    if (value) {
      highlightedIndex.value = 0;
    }
  }
);
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
    <!-- VALUE WHEN SEARCHABLE -->
    <span v-if="searchable" class="value searchable">
      <!-- IF MODEL VALUE IS NOT EMPTY, THEN SHOW IT -->
      <template v-if="modelValue.length > 0">
        <span
          v-for="option in modelValue"
          :key="option.value"
          class="value-badge"
        >
          <!-- VALUE BADGE LABEL -->
          <span>
            {{ option.label }}
          </span>
          <!-- VALUE BADGE LABEL -->

          <!-- VALUE BADGE REMOVE -->
          <button
            class="value-badge-remove"
            @click="removeOption($event, option)"
          >
            &times;
          </button>
          <!-- VALUE BADGE REMOVE -->
        </span>
        <input type="text" class="search-input" v-model="searchFilter" />
      </template>
      <!-- IF MODEL VALUE IS NOT EMPTY, THEN SHOW IT -->

      <!-- IF MODEL VALUE IS EMPTY, THEN SHOW PLACEHOLDER -->
      <template v-else>
        <span class="placeholder">Select an option</span>
      </template>
      <!-- IF MODEL VALUE IS EMPTY, THEN SHOW PLACEHOLDER -->
    </span>
    <!-- VALUE WHEN SEARCHABLE -->

    <!-- VALUE WHEN NOT SEARCHABLE -->
    <span v-else class="value">
      <!-- IF MODEL VALUE IS NOT EMPTY, THEN SHOW IT -->
      <template v-if="modelValue.length > 0">
        <span
          v-for="(option, index) in modelValue"
          :key="option.value"
          class="value-badge"
        >
          <!-- VALUE BADGE LABEL -->
          <span>
            {{ option.label }}
          </span>
          <!-- VALUE BADGE LABEL -->

          <!-- VALUE BADGE REMOVE -->
          <button
            class="value-badge-remove"
            @click="removeOption($event, option)"
          >
            &times;
          </button>
          <!-- VALUE BADGE REMOVE -->
        </span>
      </template>
      <!-- IF MODEL VALUE IS NOT EMPTY, THEN SHOW IT -->

      <!-- IF MODEL VALUE IS EMPTY, THEN SHOW PLACEHOLDER -->
      <template v-else>
        <span class="placeholder">Select an option</span>
      </template>
      <!-- IF MODEL VALUE IS EMPTY, THEN SHOW PLACEHOLDER -->
    </span>
    <!-- VALUE WHEN NOT SEARCHABLE -->

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
  @apply relative flex items-center gap-3 rounded-md bg-white p-3 outline-none;
}
.value {
  @apply flex flex-grow overflow-x-auto scroll-smooth;
}
.value.searchable {
  @apply flex-grow;
}
.value-badge:hover {
  @apply bg-red-100 text-red-500;
}
.value-badge {
  @apply m-1 flex rounded-md bg-blue-100 px-2 py-1 text-blue-500;
}
.search-input {
  @apply flex-grow bg-transparent text-gray-500 outline-none;
}
.value-badge-remove {
  @apply ml-1 cursor-pointer text-blue-500;
}
.value-badge:hover .value-badge-remove {
  @apply text-red-500;
}

.placeholder {
  @apply text-gray-500;
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
  @apply absolute left-0 top-full my-2 hidden max-h-60 w-full overflow-y-auto rounded-md bg-white p-0;
}

.options.show {
  @apply block;
}

.option {
  @apply cursor-pointer p-3 text-gray-700;
}

.option.selected {
  @apply bg-blue-100;
}
.option.highlighted {
  @apply bg-blue-500 text-white;
}

::-webkit-scrollbar {
  @apply h-0 w-0 bg-transparent;
}
</style>
