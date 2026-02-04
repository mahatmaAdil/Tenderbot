<script setup>
const emit = defineEmits(['open-lead'])
</script>

<template>
  <section class="">
    <div class="container-app">
      <div
        class="flex flex-col items-center justify-between gap-4 rounded-2xl bg-blue-600 px-6 py-5 text-white sm:flex-row"
      >
        <p class="max-w-[900px] text-sm font-medium">
          На вас работает команда закупщиков, отдел технической поддержки, юристов, которые сделают
          участие в тендерах и закупках гораздо легче, надёжнее и быстрее!
        </p>

        <button
          type="button"
          class="whitespace-nowrap rounded-full bg-white px-6 py-2 text-sm font-semibold text-slate-900 transition hover:bg-white/95"
          @click="emit('open-lead')"
        >
          Бесплатная консультация
        </button>
      </div>
    </div>
  </section>
</template>
