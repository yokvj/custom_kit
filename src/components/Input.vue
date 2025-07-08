<template>
  <div class="input-wrapper" :style="wrapperStyle">
    <input
      v-model="value"
      :placeholder="placeholder"
      @input="onInput"
      :type="type"
      class="custom-input"
      :style="inputStyle"
    />
    <p v-if="error" class="error-text">{{ error }}</p>
  </div>
</template>

<script setup>
import { ref, watch, computed } from "vue";

const props = defineProps({
  modelValue: String,
  placeholder: String,
  type: {
    type: String,
    default: "text",
  },
  // Styling props
  height: String,
  width: String,
  display: String,
  alignItems: String,
  fontSize: String,
  padding: String,
  margin: String,
  color: String,
  backgroundColor: String,
  border: String,
  borderRadius: String,

  // Validation
  validation: {
    type: Array,
    default: () => [],
  },
  errorMessages: {
    type: Object,
    default: () => ({
      required: "This field is required",
      email: "Enter a valid email",
      number: "Only numbers allowed",
      password: "Password must be at least 6 characters",
    }),
  },
});

const emit = defineEmits(["update:modelValue"]);
const value = ref(props.modelValue || "");
const error = ref("");

// Sync internal value with parent
watch(
  () => props.modelValue,
  (newVal) => {
    value.value = newVal;
  }
);

// Computed inline styles with !important
const inputStyle = computed(() => ({
  height: props.height ? `${props.height} !important` : "auto",
  width: props.width ? `${props.width} !important` : "100%",
  display: props.display || "block",
  alignItems: props.alignItems || "center",
  fontSize: props.fontSize ? `${props.fontSize} !important` : "14px",
  padding: props.padding || "8px",
  margin: props.margin || "0",
  color: props.color || "#000",
  backgroundColor: props.backgroundColor
    ? `${props.backgroundColor} !important`
    : "#fff",
  border: props.border || "1px solid #ccc",
  borderRadius: props.borderRadius || "6px",
  boxSizing: "border-box",
}));

const wrapperStyle = {
  width: "100%",
  maxWidth: "100%",
};

const validateField = () => {
  for (const rule of props.validation) {
    if (rule === "required" && !value.value.trim()) {
      return props.errorMessages.required;
    }
    if (rule === "email") {
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      if (!emailRegex.test(value.value)) {
        return props.errorMessages.email;
      }
    }
    if (rule === "number" && isNaN(value.value)) {
      return props.errorMessages.number;
    }
    if (rule === "password" && value.value.length < 6) {
      return props.errorMessages.password;
    }
  }
  return "";
};

const onInput = () => {
  error.value = validateField();
  emit("update:modelValue", value.value);
};
</script>

<style>
.custom-input {
  transition: border-color 0.3s ease;
  outline: none;
}

.custom-input:focus {
  border-color: #4a90e2 !important;
}

.error-text {
  color: #e53e3e !important;
  font-size: 12px;
  margin-top: 4px;
}

/* Responsive font fallback */
@media (max-width: 768px) {
  .custom-input {
    font-size: 13px !important;
    padding: 6px !important;
  }
}

@media (max-width: 480px) {
  .custom-input {
    font-size: 12px !important;
    padding: 5px !important;
  }
}
</style>
