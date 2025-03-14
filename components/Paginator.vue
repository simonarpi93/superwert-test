<template>
  <div class="flex gap-x-4">
    <button type="button" :disabled="!previousPageUrl" @click="setPage(previousPageUrl)">
      <IconsChevronRight class="size-4 rotate-180" />
    </button>
    <button type="button" :disabled="!nextPageUrl" @click="setPage(nextPageUrl)">
      <IconsChevronRight class="size-4" />
    </button>
  </div>
</template>

<script setup lang="ts">
// because we get the next/previous page url from BE, we can use these
defineProps<{
  previousPageUrl: string | null
  nextPageUrl: string | null
}>()

const page = defineModel<string>({ required: true })

const setPage = (link: string | null) => {
  if (!link) {
    return
  }

  page.value = link
}
</script>

<style scoped>
button {
  @apply disabled:cursor-not-allowed disabled:bg-gray-500 disabled:hover:bg-gray-500;
  @apply rounded-md bg-cyan-500 p-2 hover:bg-cyan-700;
  @apply duration-500;
}
</style>
