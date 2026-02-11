<script setup>
const emit = defineEmits(['open-lead'])
</script>

<template>
  <section>
    <div class="container-app">
      <div class="rounded-2xl bg-[#078EE6] px-6 py-5 text-white sm:px-10 sm:py-6">
        <div class="flex flex-col gap-4 sm:flex-row sm:items-center sm:justify-between sm:gap-8">
          <div class="min-w-0">
            <h3 class="text-base font-semibold leading-snug sm:text-lg">
              Участвуйте в закупках с поддержкой экспертов
            </h3>
            <p class="mt-2 text-sm font-medium text-white/80">
              Профессиональная помощь на всех этапах
            </p>
          </div>

          <button
            type="button"
            class="w-full rounded-full bg-white px-6 py-3 text-sm font-semibold text-slate-900 shadow-sm transition hover:bg-white/95 sm:w-auto sm:px-10 sm:py-4"
            @click="emit('open-lead')"
          >
            Бесплатная консультация
          </button>
        </div>
      </div>
    </div>
  </section>
</template>
