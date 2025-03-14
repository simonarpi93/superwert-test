<template>
  <div class="cursor-pointer overflow-hidden rounded-md border border-gray-200 drop-shadow-lg duration-500 hover:scale-110">
    <div class="flex flex-col justify-center gap-y-2">
      <div class="size-52">
        <!-- with nuxt image we could use loading animation -->
        <!-- this is a useful module, we can reduce the image size -->
        <NuxtImg src="https://picsum.photos/208" :width="208" :height="208" :alt="person.name" />
      </div>
      <h2 class="mx-auto text-lg font-bold">{{ person.name }}</h2>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { TPerson } from '~/utils/types/person'

defineProps<{
  person: TPerson
}>()
</script>
