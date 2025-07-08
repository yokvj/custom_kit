<template>
  <button
    :class="[
      'font-semibold focus:outline-none transition duration-300 ease-in-out',
      sizeClasses,
      bgColor || (variant === 'primary' ? 'bg-blue-500' : 'bg-gray-300'),
      textColor || (variant === 'primary' ? 'text-white' : 'text-black'),
      rounded || 'rounded-md',
      responsiveClasses,
    ]"
    @click="$emit('click')"
  >
    <slot />
  </button>
</template>

<script setup>
const props = defineProps({
  variant: {
    type: String,
    default: "primary",
  },
  bgColor: {
    type: String,
    default: "",
  },
  textColor: {
    type: String,
    default: "",
  },
  rounded: {
    type: String,
    default: "",
  },
  size: {
    type: String,
    default: "md", // Options: sm, md, lg
  },
});

const sizeClasses = computed(() => {
  switch (props.size) {
    case "sm":
      return "px-2 py-1 text-sm";
    case "lg":
      return "px-6 py-3 text-lg";
    case "md":
    default:
      return "px-4 py-2 text-base";
  }
});

const responsiveClasses = "w-full sm:w-auto"; // Button takes full width on mobile
</script>
