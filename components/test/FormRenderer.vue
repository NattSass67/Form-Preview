<!-- eslint-disable no-alert -->
<script setup>
import { reactive } from 'vue'
import CustomInput from './CustomInput.vue'
import { useRoute } from '#app'
// JSON data
// const fieldsMock = [
//   {
//     label: 'Field Name',
//     key: 'fieldKey589',
//     type: 'String',
//     required: false,
//     description: 'Description',
//     placeholder: 'Placeholder',
//     defaultValue: 'string',
//     format: 'String', // can be either Email, String
//     validation: {
//       max: 255,
//       min: 1,
//     },
//   },
//   {
//     description: 'Description',
//     key: 'fieldKey244',
//     placeholder: 'Placeholder',
//     label: 'Field Name',
//     type: 'Number',
//     required: false,
//     defaultValue: 1,
//     validation: {
//       max: 9999999,
//       min: 1,
//     },
//   },
//   {
//     description: 'Description',
//     key: 'fieldKey388',
//     placeholder: 'Placeholder',
//     label: 'Field Name',
//     type: 'Boolean',
//     required: false,
//     defaultValue: true,
//   },
//   {
//     description: 'Description',
//     key: 'fieldKey862',
//     placeholder: 'Placeholder',
//     label: 'Field Name',
//     type: 'Enum',
//     required: false,
//     enumName: 'MyEnum Name',
//     enumValues: [
//       {
//         id: '1729698606633',
//         label: 'label',
//         value: 'value',
//       },
//     ],
//   },
//   {
//     description: 'Description',
//     key: 'fieldKey998',
//     placeholder: 'Placeholder',
//     label: 'Field Name',
//     type: 'Date',
//     required: false,
//   },
// ]

// State to store the decoded form fields
const fields = ref([]) // Reactive array for form fields
const formData = reactive({}) // Reactive object for form data

// Use the route to get the query parameter
const route = useRoute()

onMounted(() => {
  // Check if 'data' query parameter exists
  if (route.query.data) {
    try {
      // Decode and parse the JSON data from the query parameter
      const parsedFields = JSON.parse(decodeURIComponent(route.query.data))

      // Assign fields and initialize formData with default values
      fields.value = parsedFields
      parsedFields.forEach((field) => {
        formData[field.key] = field.defaultValue || '' // Initialize formData with default values
      })
    }
    catch (error) {
      console.error('Error decoding form data:', error)
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
        v-for="field in fields"
        :id="field.key"
        :key="field.key"
        v-model="formData[field.key]"
        :label="field.label"
        :type="field.type"
        :placeholder="field.placeholder"
        :options="field.enumValues || []"
        :description="field.description"
        :required="field.required"
        :default-value="field.defaultValue ? field.defaultValue : ''"
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
