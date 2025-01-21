<!-- eslint-disable unused-imports/no-unused-vars -->
<!-- eslint-disable no-alert -->
<script setup>
import { reactive } from 'vue'
import CustomInput from './CustomInput.vue'
import { useRoute } from '#app'
// JSON data
const fieldsMock = [
  {
    label: 'Test23',
    key: 'fieldKey647',
    type: 'String',
    required: false,
    description: 'Description',
    placeholder: 'Placeholder',
    defaultValue: 'string',
    format: 'String',
    validation: {
      max: 255,
      min: 1,
    },
  },
  {
    description: 'Description',
    key: 'fieldKey578',
    placeholder: 'Placeholder',
    label: 'Field Name555',
    type: 'Number',
    required: false,
    defaultValue: 1,
    validation: {
      max: 9999999,
      min: 1,
    },
  },
  {
    description: 'Description',
    key: 'fieldKey179',
    placeholder: 'Placeholder',
    label: 'Field Name',
    type: 'Boolean',
    required: false,
    defaultValue: true,
  },
  {
    description: 'Description',
    key: 'fieldKey974',
    placeholder: 'Placeholder',
    label: 'Field Name',
    type: 'Enum',
    required: false,
    enumName: 'MyEnum Name',
    enumValues: [
      {
        id: '1737452960109',
        label: 'label2',
        value: 'value',
      },
      {
        id: '1737452980182',
        label: 'New Label5',
        value: 'New Value',
      },
    ],
    enumStyle: 'Select',
  },
  {
    description: 'Description',
    key: 'fieldKey335',
    placeholder: 'Placeholder',
    label: 'Field Name',
    type: 'Date',
    required: false,
  },
]

// State to store the decoded form fields
const fields = ref([]) // Reactive array for form fields
const formData = reactive({}) // Reactive object for form data

// Use the route to get the query parameter
const route = useRoute()

onMounted(async () => {
  // Check if 'id' parameter exists
  if (route.params.id) {
    try {
      // Fetch the data from the API using the 'id' parameter
      const config = useRuntimeConfig()
      const res = await fetch(`/api/link/query?slug=${route.params.id}`, {
        headers: {
          Authorization: `Bearer ${config.public.apiToken}`,
        },
      })

      // Parse the JSON data from the response
      const parsedFields = await res.json()
      console.log('parsed', parsedFields)
      // Assign fields and initialize formData with default values
      fields.value = parsedFields.field_data
      parsedFields.field_data.forEach((field) => {
        formData[field.key] = field.defaultValue || '' // Initialize formData with default values
      })
    }
    catch (error) {
      console.error('Error fetching form data:', error)
    }
  }
})

function handleSubmit() {
  alert(JSON.stringify(formData))
  // Perform any action on form submit
}
</script>

<template>
  <div v-if="fields.length > 0">
    <h1 class="text-2xl font-bold mb-4">
      Form Preview
    </h1>
    <form class="space-y-6" @submit.prevent="handleSubmit">
      <CustomInput
        v-for="field in fields" :id="field.key" :key="field.key" v-model="formData[field.key]"
        :label="field.label" :type="field.type" :placeholder="field.placeholder" :options="field.enumValues || []"
        :description="field.description" :required="field.required"
        :default-value="field.defaultValue ? field.defaultValue : ''"
        :enum-style="field.enumStyle ? field.enumStyle.toLowerCase() : ''"
      />

      <Button type="submit">
        Submit
      </Button>
    </form>
  </div>
  <div v-else>
    Loading form...
  </div>
</template>
