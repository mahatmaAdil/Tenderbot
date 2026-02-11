<script setup>
const emit = defineEmits(['open-lead'])
</script>

<template>
  <section class="relative overflow-hidden sm:py-14">
    <img
      src="@/assets/cta/card-cta.svg"
      alt=""
      class="pointer-events-none absolute left-1/2 top-0 w-[1922px] max-w-none -translate-x-1/2"
    />

    <div class="relative container-app flex items-center sm:block">
      <div
        class="mx-auto w-full max-w-[360px] px-6 py-7 text-center text-white sm:max-w-[990px] sm:px-10 sm:py-5"
      >
        <h2 class="text-2xl sm:text-5xl leading-[1.25]">
          Прямо сейчас более
          <span class="font-bold leading-[64px]">1000 </span>
          <span class="block sm:inline">актуальных тендеров </span>
          <span class="block sm:inline">по вашей нише</span>
        </h2>

        <p
          class="mx-auto mt-2 max-w-[280px] text-[12px] leading-[1.25] text-white/85 sm:mt-4 sm:max-w-[700px] sm:text-[16px] sm:leading-[1.4]"
        >
          Покажем крупные тендеры и объясним, как начать работать с сервисом.
        </p>

        <div class="mt-4 flex justify-center sm:mt-8">
          <button
            type="button"
            class="inline-flex h-[64px] w-full max-w-[280px] items-center justify-center rounded-full bg-white px-6 py-3 text-[20px] font-bold text-slate-900 shadow-lg shadow-black/10 transition hover:-translate-y-[1px] hover:bg-white/95 active:translate-y-0 sm:w-auto sm:max-w-none sm:px-8"
            @click="emit('open-lead')"
          >
            <span class="hidden sm:inline">Хочу посмотреть</span>
            <span class="inline sm:hidden">Узнать больше</span>
          </button>
        </div>
      </div>
    </div>
  </section>
</template>
