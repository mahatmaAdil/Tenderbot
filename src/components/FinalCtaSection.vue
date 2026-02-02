<script setup>
const emit = defineEmits(["open-lead"]);
</script>

<template>
  <section class="py-14">
    <div class="container-app">
      <div class="card p-6 flex flex-col gap-4 sm:flex-row sm:items-center sm:justify-between">
        <div>
          <div class="text-lg font-extrabold text-slate-900">Прямо сейчас более 1000 актуальных тендеров по вашей нише.</div>
          <div class="mt-1 text-sm text-slate-600">Покажем крупные тендеры и объясним, как начать работать с сервисом.</div>
        </div>
        <button class="btn-primary" @click="emit('open-lead')">Хочу посмотреть</button>
      </div>
    </div>
  </section>
</template>
