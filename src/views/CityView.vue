<template>
  <div>
    <Suspense>
      <WeatherHandle />
      <template #fallback>
        <SavedCitiesLayout />
      </template>
    </Suspense>
  </div>
</template>

<script setup>
import WeatherHandle from "../components/WeatherHandle.vue";
import SavedCitiesLayout from "../components/SavedCitiesLayout.vue";
</script>
