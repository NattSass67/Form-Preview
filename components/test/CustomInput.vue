<!-- eslint-disable unused-imports/no-unused-vars -->

<script>
import {
  computed,
  defineEmits,
  defineProps,
} from 'vue'

const props = defineProps({
  modelValue: [String, Number], // Allow different input types
  label: String,
  id: String,
  type: {
    type: String,
    default: 'text',
  },
  placeholder: String,
})

const emits = defineEmits(['update:modelValue'])

// Create a computed property for v-model compatibility
const inputValue = computed({
  get: () => props.modelValue,
  set: value => emits('update:modelValue', value),
})

// Function to emit input event with updated value
function updateValue(event) {
  emits('update:modelValue', event.target.value)
}
</script>

<template>
  <div>
    <label :for="id" class="block text-sm font-medium text-gray-700 mb-1">{{ label }}</label>
    <input :id="id" v-model="inputValue" :type="type" :placeholder="placeholder" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" @input="updateValue">
  </div>
</template>

<style scoped>
/* Custom styles if needed */
</style>
