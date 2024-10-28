<script setup>
import { computed, defineEmits, defineProps } from 'vue'
import { AutoFormFieldBoolean } from '../ui/auto-form'

const props = defineProps({
  modelValue: [String, Number, Boolean, Object],
  label: String,
  id: String,
  type: String,
  placeholder: String,
  description: String, // Description prop
  required: Boolean,
  options: { type: Array, default: () => [] }, // For Enum options,
  validation: Object,
  defaultValue: [String, Number, Boolean],
})

const emits = defineEmits(['update:modelValue'])

// Computed property to enable two-way binding with `v-model`
const inputValue = computed({
  get: () => props.modelValue,
  set: value => emits('update:modelValue', value),
})
</script>

<template>
  <div>
    <!-- String and Number Inputs -->
    <AutoFormFieldInput
      v-if="type === 'String'" v-model="inputValue" :field-name="props.id" :label="props.label"
      :required="props.required"
      :disabled="false"
      :config="{ description: props.description ? props.description : '', inputProps: { placeholder: props.placeholder } }"
    />

    <!-- Checkbox for Boolean type -->
    <AutoFormFieldBoolean
      v-if="type === 'Boolean'" v-model="inputValue" :field-name="props.id" :label="props.label"
      :required="props.required" :disabled="false"
      :config="{ description: props.description ? props.description : '', inputProps: { placeholder: props.placeholder }, component: 'switch' }"
    />

    <!-- Select Dropdown for Enum type -->
    <AutoFormFieldEnum
      v-if="type === 'Enum'" v-model="inputValue" :field-name="props.id" :label="props.label" :required="props.required"
      :disabled="false"
      :config="{ description: props.description ? props.description : '', inputProps: { placeholder: props.placeholder } }"
      :options="props.options.map((object) => object.value)"
    />

    <AutoFormFieldNumber
      v-if="type === 'Number'" v-model="inputValue" :field-name="props.id" :label="props.label" :required="props.required"
      :disabled="false"
      :default-value="props.defaultValue"
      :config="{ description: props.description ? props.description : '', inputProps: { placeholder: props.placeholder, max: props.validation?.max, min: props.validation?.min } }"
    />

    <!-- Date Picker for Date type -->
    <AutoFormFieldDate
      v-if="type === 'Date'" v-model="inputValue" :field-name="props.id" :label="props.label" :required="props.required"
      :disabled="false"
      :config="{ description: props.description ? props.description : '', inputProps: { placeholder: props.placeholder } }"
    />
  </div>
</template>
