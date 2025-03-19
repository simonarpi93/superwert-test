<template>
  <h1 class="text-center text-xl font-bold">Star wars characters ({{ data?.count }})</h1>

  <!-- show a loading indicator while we are waiting -->
  <LoadingIndicator v-if="status !== 'success'" />
  <div v-if="!!data && status === 'success'">
    <div class="flex justify-end">
      <Search v-model="search" />
    </div>

    <div class="my-5 flex flex-wrap justify-center gap-6">
      <PeopleCard v-for="(person, index) in data.results" :key="index" v-memo="[person.name]" :person @click="showDetails = person" />
    </div>

    <div class="flex justify-center">
      <Paginator v-model="page" :next-page-url="data.next" :previous-page-url="data.previous" />
    </div>
  </div>

  <Transition :duration="500">
    <LazyModalsPersonDetails v-if="showDetails" :person="showDetails" @close="showDetails = undefined" />
  </Transition>
</template>

<script setup lang="ts">
import type { TPerson } from '~/utils/types/person'
import type { IFetchResult } from '~/utils/interfaces/general'

const sessionStoragePageEntry = 'etradingPage'

// show modal indicator
const showDetails = shallowRef()

// the default endpoint URL
const initUrl = 'https://swapi.dev/api/people'

// the current requested page. It is being changed by the paginator
const page = shallowRef(sessionStorage.getItem(sessionStoragePageEntry) ?? initUrl)

const searchParamInRequestUrl = computed((): string | null => {
  return new URL(page.value)?.searchParams?.get('search')
})

// search params
const search = shallowRef(searchParamInRequestUrl.value ?? '')

// first page indicator. If the search value changed, we set it to true, and set the page url to default
const isFirstPage = shallowRef(true)
watch(search, () => {
  isFirstPage.value = true
  page.value = initUrl
})

const { data, status } = useLazyAsyncData(
  'people-list',
  async (): Promise<IFetchResult<Array<TPerson>>> => {
    try {
      // if the search param is not empty, and the first page indicator is true, we send a request with the search query
      // otherwise, we use the next/previous url (we got it in the response)
      const requestedPage = !!search.value?.length && isFirstPage.value && !searchParamInRequestUrl.value ? `${initUrl}/?search=${search.value}` : page.value
      const response: IFetchResult<Array<TPerson>> = await $fetch(requestedPage)

      // if we got the response, we set the first page indicator to false
      isFirstPage.value = false

      sessionStorage.setItem(sessionStoragePageEntry, requestedPage)

      return response
    } catch (error) {
      console.error(error)
      // if something went wrong, show the error page
      throw showError({
        fatal: true,
        message: 'An error occurred while trying to fetch data',
      })
    }
  },
  // we are watching the search and page values, if one of those are changed, send a new request
  { watch: [search, page], deep: false },
)
</script>
