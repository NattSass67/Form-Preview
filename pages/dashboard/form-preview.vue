<script setup>
import FormRenderer from '~/components/test/FormRenderer.vue'

provide('id', ref())
</script>

<template>
  <main class="space-y-6">
    <DashboardBreadcrumb title="Analysis" />
    <FormRenderer />
  </main>
</template>
