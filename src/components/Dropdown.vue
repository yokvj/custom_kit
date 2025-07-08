<template>
  <div class="w-full">
    <select
      v-model="selected"
      @change="onChange"
      :class="[
        'rounded outline-none transition duration-300 ease-in-out w-full',
        selectClasses,
        error ? 'border-red-500' : 'border-gray-300',
      ]"
    >
      <option disabled value="">Select an option</option>
      <option v-for="opt in options" :key="opt.value" :value="opt.value">
        {{ opt.label }}
      </option>
    </select>
    <p v-if="error" class="text-red-500 text-sm mt-1">{{ error }}</p>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";

const props = defineProps({
  modelValue: {
    type: String,
    default: "",
  },
  options: {
    type: Array,
    required: true,
  },
  validation: {
    type: Array,
    default: () => [], // e.g., ['required', 'number']
  },
  errorMessages: {
    type: Object,
    default: () => ({
      required: "This field is required",
      number: "Only numbers allowed",
    }),
  },
  selectClasses: {
    type: String,
    default: "p-2 border text-sm sm:text-base",
  },
});

const emit = defineEmits(["update:modelValue", "change"]);
const selected = ref(props.modelValue || "");
const error = ref("");

// Sync external modelValue
watch(
  () => props.modelValue,
  (newVal) => {
    selected.value = newVal;
  }
);

// Validation logic
const validateField = () => {
  for (const rule of props.validation) {
    if (rule === "required" && !selected.value) {
      return props.errorMessages.required;
    }

    if (rule === "number" && isNaN(selected.value)) {
      return props.errorMessages.number;
    }
  }
  return "";
};

const onChange = () => {
  error.value = validateField();
  emit("update:modelValue", selected.value);
  emit("change", selected.value);
};
</script>
