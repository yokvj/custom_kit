<template>
  <div class="w-full">
    <label class="inline-flex items-start space-x-2">
      <input
        type="checkbox"
        v-model="checkedValue"
        @change="onChange"
        :class="['form-checkbox mt-1', checkboxClasses]"
      />
      <span class="text-sm sm:text-base"><slot /></span>
    </label>
    <p v-if="error" class="text-red-500 text-sm mt-1">{{ error }}</p>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";

const props = defineProps({
  modelValue: {
    type: Boolean,
    default: false,
  },
  validation: {
    type: Array,
    default: () => [], // e.g., ['required']
  },
  errorMessages: {
    type: Object,
    default: () => ({
      required: "This checkbox must be checked",
    }),
  },
  checkboxClasses: {
    type: String,
    default: "h-4 w-4 text-blue-600 border-gray-300 rounded",
  },
});

const emit = defineEmits(["update:modelValue", "change"]);
const checkedValue = ref(props.modelValue);
const error = ref("");

// Sync with external modelValue
watch(
  () => props.modelValue,
  (newVal) => {
    checkedValue.value = newVal;
  }
);

const validateField = () => {
  for (const rule of props.validation) {
    if (rule === "required" && !checkedValue.value) {
      return props.errorMessages.required;
    }
  }
  return "";
};

const onChange = () => {
  error.value = validateField();
  emit("update:modelValue", checkedValue.value);
  emit("change", checkedValue.value);
};
</script>
