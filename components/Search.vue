<template>
  <div class="group flex items-center gap-x-2 rounded-md border p-0.5">
    <input type="search" class="peer order-2 size-full focus:outline-none focus:ring-0" placeholder="Search" :value="text" @change="onChange" />
    <IconsSearch class="order-1 block size-4 cursor-pointer transition-all duration-300 peer-focus:w-0 peer-focus:scale-0" />
  </div>
</template>

<script setup lang="ts">
const text = defineModel<string>({ required: false })

// we dont want to send a request everytime when the input value changes, so we handle it with manually
// otherwise we could use the v-model directive on the input, but we have to implement a debounce logic (this will be the better solution)
const onChange = ($event: Event) => {
  text.value = ($event.target as HTMLInputElement).value
}
</script>
