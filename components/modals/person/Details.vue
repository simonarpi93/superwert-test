<template>
  <div class="fixed inset-0 flex items-center justify-center bg-black/80">
    <div class="h-96 w-96">
      <div class="flex h-full flex-col overflow-auto rounded-md bg-white p-3 md:p-6">
        <div v-once class="mb-2 flex items-center justify-between">
          <h3 class="text-lg font-bold">{{ person.name }}</h3>

          <div class="flex items-center gap-x-2">
            <IconsCrossMark class="size-5 cursor-pointer" @click="$emit('close')" />
          </div>
        </div>

        <div>
          <h4 class="font-bold">Personal data</h4>
          <div class="data-container ml-2">
            <span><strong>Height:</strong> {{ person.height }}</span>
            <span><strong>Mass:</strong> {{ person.mass }}</span>
            <span><strong>Birth year:</strong> {{ person.birth_year }}</span>
            <span><strong>Height:</strong> {{ person.height }}</span>
            <span><strong>Appeared in films</strong> {{ person.films.length }}</span>
          </div>

          <div class="mt-3">
            <!-- show loading indicator while we are waiting -->
            <LoadingIndicator v-if="status === 'pending'" class="size-4" />
            <!-- if something went wrong, show an error message -->
            <strong v-if="status === 'error'" class="underline">An error occurred while trying to fetch person planet data</strong>

            <template v-if="data && status === 'success'">
              <h4 class="font-bold">Planet data</h4>
              <div class="data-container ml-2">
                <span><strong>Name:</strong> {{ data.name }}</span>
                <span><strong>Terrain:</strong> {{ data.terrain }}</span>
                <span><strong>Climate:</strong> {{ data.climate }}</span>
              </div>
            </template>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { TPerson } from '~/utils/types/person'
import type { TPlanet } from '~/utils/types/Planet'

const { person } = defineProps<{
  person: TPerson
}>()

// fetch planet data
const { data, status } = useAsyncData('person-planet', (): Promise<TPlanet> => $fetch(person.homeworld), { deep: false })

defineEmits<{
  close: () => void
}>()
</script>

<style scoped>
.data-container {
  @apply gap-y-4 *:block;
}
</style>
