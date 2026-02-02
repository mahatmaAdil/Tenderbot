<script setup>
const emit = defineEmits(["open-lead"]);
</script>

<template>
  <section class="py-14">
    <div class="container-app">
      <div class="flex items-center justify-between gap-4">
        <div class="text-slate-900 font-bold">How it works (3 шага)</div>
        <button class="btn-primary" @click="emit('open-lead')">Попробовать сейчас</button>
      </div>
    </div>
  </section>
</template>
