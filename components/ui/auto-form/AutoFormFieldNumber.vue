<script setup lang="ts">
import AutoFormLabel from './AutoFormLabel.vue'
import { beautifyObjectName } from './utils'
import type { FieldProps } from './interface'
import { FormControl, FormDescription, FormField, FormItem, FormMessage } from '@/components/ui/form'
import { Input } from '@/components/ui/input'

defineOptions({
  inheritAttrs: false,
})


//kanoon-custom-section
const props = defineProps<FieldProps & { modelValue: string | number, defaultValue: string | number }>()
const emit = defineEmits(['update:modelValue']);
const passValue = computed({
  get() {
    return props.modelValue;
  },
  set(value) {
    emit('update:modelValue', value);
  },
});
</script>

<template>
  <FormField v-slot="slotProps" :name="fieldName">
    <FormItem>
      <AutoFormLabel v-if="!config?.hideLabel" :required="required">
        {{ config?.label || beautifyObjectName(label ?? fieldName) }}
      </AutoFormLabel>
      <FormControl>
        <slot v-bind="slotProps">
          <Input type="number" v-model="passValue" :default-value="props.defaultValue"
            v-bind="{ ...slotProps.componentField, ...config?.inputProps }" :disabled="disabled" />
        </slot>
      </FormControl>
      <FormDescription v-if="config?.description">
        {{ config.description }}
      </FormDescription>
      <FormMessage />
    </FormItem>
  </FormField>
</template>
