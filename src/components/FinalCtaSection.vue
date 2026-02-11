<script setup>
const emit = defineEmits(['open-lead'])
</script>

<template>
  <section class="relative min-h-[240px] overflow-hidden py-10 sm:py-14">
    <img
      src="@/assets/cta/card-cta.svg"
      alt=""
      class="pointer-events-none absolute left-1/2 top-0 w-[1922px] max-w-none -translate-x-1/2"
    />

    <div class="relative container-app flex items-center justify-center text-center">
      <div class="mx-auto w-full max-w-[990px] px-6 py-7 text-white sm:px-10 sm:py-10">
        <h2 class="text-[28px] font-semibold leading-[1.3] sm:text-[48px] sm:leading-[1.2]">
          Прямо сейчас более <span class="font-extrabold">1000</span>
          актуальных тендеров по вашей нише
        </h2>

        <p
          class="mx-auto mt-6 max-w-[750px] text-[14px] leading-[1.35] text-white/85 sm:mt-8 sm:text-[20px]"
        >
          Покажем крупные тендеры и объясним, как начать работать с сервисом.
        </p>

        <div class="mt-8 flex justify-center">
          <button
            type="button"
            class="inline-flex h-[64px] items-center justify-center rounded-full bg-white px-8 text-[20px] font-bold text-slate-900 shadow-lg shadow-black/10 transition hover:-translate-y-[1px] hover:bg-white/95 active:translate-y-0"
            @click="emit('open-lead')"
          >
            Хочу посмотреть
          </button>
        </div>
      </div>
    </div>
  </section>
</template>
