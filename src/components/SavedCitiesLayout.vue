<template>
  <div class="flex flex-col flex-1">
    <!-- Overview -->
    <div class="flex flex-col py-12 items-center">
      <Animations class="max-w-[300px] w-full mb-2" />
      <Animations class="max-w-[300px] w-full mb-12" />
      <Animations class="max-w-[300px] h-[100px] w-full mb-12" />
      <Animations class="max-w-[300px] w-full mb-8" />
      <Animations class="max-w-[300px] h-[75px] w-full" />
    </div>
    <!-- Hourly -->
    <div class="flex flex-col py-12 px-8 items-center">
      <Animations class="max-w-screen-md h-[100px] w-full mb-12" />
    </div>
    <!-- Weekly -->
    <div class="flex flex-col py-12 px-8 items-center">
      <Animations class="max-w-screen-md h-[100px] w-full mb-12" />
    </div>
  </div>
</template>

<script setup>
import Animations from "./Animations.vue";
</script>
