<template>
  <NuxtLayout>
    <strong class="mt-4 block w-full text-center">
      <template v-if="typeof error === 'string'">
        {{ error }}
      </template>
      <template v-else>
        {{ error?.message }}
      </template>
    </strong>
  </NuxtLayout>
</template>

<script setup lang="ts">
import type { NuxtError } from 'nuxt/app'

defineProps<{
  error?: NuxtError | string
}>()
</script>
