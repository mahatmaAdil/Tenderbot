<script setup>
const emit = defineEmits(['open-lead'])
</script>

<template>
  <section
    class="relative overflow-hidden bg-gradient-to-br from-sky-500 via-blue-600 to-indigo-700 text-white"
  >
    <!-- декор: точки -->
    <div
      aria-hidden="true"
      class="pointer-events-none absolute inset-0 opacity-20"
      style="
        background-image:
          radial-gradient(circle at 20% 20%, rgba(255, 255, 255, 0.35) 0 2px, transparent 3px),
          radial-gradient(circle at 80% 30%, rgba(255, 255, 255, 0.25) 0 2px, transparent 3px),
          radial-gradient(circle at 60% 75%, rgba(255, 255, 255, 0.25) 0 2px, transparent 3px);
        background-size: 180px 180px;
      "
    />

    <!-- декор: мягкое свечение -->
    <div
      aria-hidden="true"
      class="pointer-events-none absolute -top-32 left-1/2 h-[520px] w-[820px] -translate-x-1/2 rounded-full bg-white/10 blur-3xl"
    />

    <div class="container-app relative py-16 sm:py-20 lg:py-24">
      <div class="mx-auto max-w-[820px] text-center">
        <h1 class="text-3xl font-extrabold leading-tight sm:text-4xl lg:text-5xl">
          Tenderbot.kz – тендеры и госзакупки Казахстана в одном окне
        </h1>

        <p class="mx-auto mt-4 max-w-[640px] text-sm text-white/85 sm:text-base">
          Находите подходящие тендеры без ручного поиска.
          <br class="hidden sm:block" />
          Сервис автоматически подберет по вашим параметрам на 142 площадках.
        </p>

        <div class="mx-auto mt-8 flex justify-center">
          <button
            type="button"
            class="inline-flex items-center justify-center rounded-full bg-white px-8 py-3 text-sm font-semibold text-slate-900 shadow-lg shadow-black/10 transition hover:-translate-y-[1px] hover:bg-white/95 active:translate-y-0"
            @click="emit('open-lead')"
          >
            Узнать больше
          </button>
        </div>
      </div>
    </div>
  </section>
</template>
