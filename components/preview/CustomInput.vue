<script setup>
import { computed, defineEmits, defineProps } from 'vue'
import { AutoFormFieldBoolean } from '../ui/auto-form'

const props = defineProps({
  modelValue: [String, Number, Boolean, Object], // input field value???
  label: String,
  id: String,
  type: String,
  placeholder: String,
  description: String, // Description prop
  required: Boolean,
  options: { type: Array, default: () => [] }, // For Enum options,
  validation: Object,
  defaultValue: [String, Number, Boolean],
  enumStyle: String,
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
    <div v-if="type === 'String'" class="max-w-96">
      <AutoFormFieldInput
        v-if="type === 'String'" v-model="inputValue" :field-name="props.id" :label="props.label"
        :required="props.required" :disabled="false"
        :config="{ description: props.description ? props.description : '', inputProps: { placeholder: props.placeholder } }"
      />
    </div>

    <!-- Checkbox for Boolean type -->
    <div v-if="type === 'Boolean'" class="max-w-96">
      <AutoFormFieldBoolean
        v-if="type === 'Boolean'" v-model="inputValue" :field-name="props.id" :label="props.label"
        :required="props.required" :disabled="false"
        :config="{ description: props.description ? props.description : '', inputProps: { placeholder: props.placeholder }, component: 'switch' }"
      />
    </div>

    <!-- Select Dropdown for Enum type -->
    <div v-if="type === 'Enum'" class="max-w-96">
      <AutoFormFieldEnum
        v-if="type === 'Enum'" v-model="inputValue" :field-name="props.id" :label="props.label"
        :required="props.required" :disabled="false"
        :config="{ description: props.description ? props.description : '', inputProps: { placeholder: props.placeholder }, component: props.enumStyle }"
        :options="props.options.map((object) => object.value)"
      />
    </div>

    <div v-if="type === 'Number'" class="max-w-96">
      <AutoFormFieldNumber
        v-if="type === 'Number'" v-model="inputValue" :field-name="props.id" :label="props.label"
        :required="props.required" :disabled="false" :default-value="props.defaultValue"
        :config="{ description: props.description ? props.description : '', inputProps: { placeholder: props.placeholder, max: props.validation?.max, min: props.validation?.min } }"
      />
    </div>

    <!-- Date Picker for Date type -->
    <div v-if="type === 'Date'" class="max-w-96">
      <AutoFormFieldDate
        v-if="type === 'Date'" v-model="inputValue" :field-name="props.id" :label="props.label"
        :required="props.required" :disabled="false"
        :config="{ description: props.description ? props.description : '', inputProps: { placeholder: props.placeholder } }"
      />
    </div>
  </div>
</template>
