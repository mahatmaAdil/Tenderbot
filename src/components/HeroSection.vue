<script setup>
const emit = defineEmits(['open-lead'])
import bgShapeUrl from '../assets/bg-shape.svg'
</script>

<template>
  <section class="relative overflow-hidden text-white">
    <div
      aria-hidden="true"
      class="absolute inset-0"
      style="
        background:
          radial-gradient(900px 520px at 50% 10%, rgba(255, 255, 255, 0.18), transparent 60%),
          linear-gradient(135deg, #38bdf8 0%, #2563eb 45%, #4338ca 100%);
      "
    />

    <div
      aria-hidden="true"
      class="pointer-events-none absolute inset-0 opacity-[0.75] mix-blend-overlay"
      :style="{
        backgroundImage: `url(${bgShapeUrl})`,
        backgroundRepeat: 'no-repeat',
        backgroundSize: 'cover',
        backgroundPosition: 'center',
      }"
    ></div>
    <div
      aria-hidden="true"
      class="pointer-events-none absolute inset-0 opacity-[0.75] mix-blend-overlay"
      :style="{
        backgroundImage: `url(${bgShapeUrl})`,
        backgroundRepeat: 'no-repeat',
        backgroundSize: 'cover',
        backgroundPosition: 'left center',
        transform: 'scaleX(-1)',
      }"
    ></div>

    <div
      aria-hidden="true"
      class="pointer-events-none absolute -top-36 left-1/2 h-[520px] w-[820px] -translate-x-1/2 rounded-full bg-white/10 blur-3xl"
    ></div>

    <div class="container-app relative py-16 sm:py-20 lg:py-24">
      <div class="mx-auto max-w-[820px] text-center">
        <h1 class="text-3xl font-extrabold leading-tight sm:text-4xl lg:text-5xl">
          Tenderbot.kz - тендеры и госзакупки Казахстана в одном окне
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
