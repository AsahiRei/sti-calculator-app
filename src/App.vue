<template>
  <div class="min-h-screen flex flex-col bg-gray-100">
    <main class="flex-1 container mx-auto max-w-5xl py-4 px-2">
      <Description />
      <Calculation />
      <Grades />
    </main>
    <footer class="bg-gray-800 py-4 px-2">
      <p class="text-center text-white text-sm sm:text-base md:text-lg">
        &copy; {{ new Date().getFullYear() - 1 }} -
        {{ new Date().getFullYear() }} ArnDev. All rights reserved.
      </p>
    </footer>
  </div>
</template>
<script setup>
import Description from "./components/Description.vue";
import Calculation from "./components/Calculation.vue";
import Grades from "./components/Grades.vue";
</script>
