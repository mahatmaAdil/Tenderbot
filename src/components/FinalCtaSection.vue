<script setup>
const emit = defineEmits(['open-lead'])
import bg from '../assets/cta/card-cta.svg'
</script>

<template>
  <section class="bg-white">
    <div class="container-app py-14 sm:py-18">
      <div
        class="relative overflow-hidden rounded-3xl px-6 py-14 text-center text-white sm:px-10"
        :style="{
          backgroundImage: `url(${bg})`,
          backgroundSize: 'cover',
          backgroundPosition: 'center',
        }"
      >
        <h2 class="text-2xl font-extrabold sm:text-3xl">
          Прямо сейчас более <span class="text-white">1000</span><br />
          актуальных тендеров по вашей нише
        </h2>

        <p class="mx-auto mt-4 max-w-[520px] text-sm text-white/85">
          Покажем крупные тендеры и объясним, как начать работать с сервисом.
        </p>

        <div class="mt-8 flex justify-center">
          <button
            type="button"
            class="rounded-full bg-white px-8 py-3 text-sm font-semibold text-slate-900 transition hover:bg-white/95"
            @click="emit('open-lead')"
          >
            Хочу посмотреть
          </button>
        </div>
      </div>
    </div>
  </section>
</template>
