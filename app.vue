<template>
  <NuxtLoadingIndicator :throttle="0" color="#00ffeb" :duration="50000" />
  <div class="mx-auto max-w-6xl py-4">
    <NuxtRouteAnnouncer />
    <NuxtPage />
  </div>
</template>
