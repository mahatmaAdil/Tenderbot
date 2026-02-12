<script setup>
import bgShapeUrl from '@/assets/bg-shape3.svg'
import bgShapeUrl2 from '@/assets/bg-shape2.svg'
import { ref, onMounted, onUnmounted } from 'vue'
const emit = defineEmits(['open-lead'])

const hero = ref(null)
const isOverHero = ref(true)

let observer

onMounted(() => {
  observer = new IntersectionObserver(
    ([entry]) => {
      isOverHero.value = entry.isIntersecting
    },
    {
      root: null,
      threshold: 0,
    },
  )

  if (hero.value) observer.observe(hero.value)
})

onUnmounted(() => {
  if (observer && hero.value) observer.unobserve(hero.value)
})
</script>

<template>
  <section ref="hero" class="relative overflow-hidden text-white">
    <div
      aria-hidden="true"
      class="absolute inset-0"
      style="background: linear-gradient(135deg, #00a6f4 0%, #0084d1 45%, #1447e6 100%)"
    />

    <img
      :src="bgShapeUrl"
      aria-hidden="true"
      class="pointer-events-none absolute left-1/2 top-1/2 hidden sm:block max-w-none -translate-x-1/2 -translate-y-1/2 opacity-100 mix-blend-overlay sm:w-[1400px] sm:left-[20%] sm:opacity-95"
      alt=""
    />

    <img
      :src="bgShapeUrl"
      aria-hidden="true"
      class="pointer-events-none absolute left-1/2 top-1/2 w-[900px] max-w-none -translate-x-1/2 -translate-y-1/2 rotate-180 opacity-99 mix-blend-overlay sm:w-[1200px] sm:left-[65%] sm:opacity-30 lg:w-[1400px] lg:left-[80%] lg:opacity-35"
      alt=""
    />

    <div
      aria-hidden="true"
      class="pointer-events-none absolute -top-36 left-1/2 h-[520px] w-[820px] -translate-x-1/2 rounded-full bg-white/10 blur-3xl"
    ></div>

    <header
      class="fixed top-4 left-1/2 z-50 -translate-x-1/2 hidden sm:block"
      :class="isOverHero ? 'text-white' : 'text-slate-900'"
    >
      <nav
        class="mx-auto flex items-center justify-center gap-6 sm:gap-8 lg:gap-12 rounded-full bg-white/15 px-5 py-2 sm:px-8 sm:py-3 lg:px-12 lg:py-4 backdrop-blur-md ring-1 ring-white/20 transition-colors min-w-[1072px]"
      >
        <a
          href="#tenders"
          class="text-[16px] mr-8 font-medium transition-colors hover:text-[#0000]"
          :class="
            isOverHero ? 'text-white/90 hover:text-black' : 'text-slate-700 hover:text-slate-900'
          "
        >
          Актуальные тендеры
        </a>
        <a
          href="#about"
          class="text-[16px] mr-8 font-medium transition-colors nav-link hover:text-[#0000]"
          :class="
            isOverHero ? 'text-white/90 hover:text-black' : 'text-slate-700 hover:text-slate-900'
          "
        >
          О сервисе
        </a>
        <a
          href="#how"
          class="text-[16px] mr-8 font-medium transition-colors hover:text-[#0000]"
          :class="
            isOverHero ? 'text-white/90 hover:text-black' : 'text-slate-700 hover:text-slate-900'
          "
        >
          Как это работает
        </a>
        <a
          href="#form"
          class="text-[16px] mr-8 font-medium transition-colors hover:text-[#0000]"
          :class="
            isOverHero ? 'text-white/90 hover:text-black' : 'text-slate-700 hover:text-slate-900'
          "
        >
          Оставить заявку
        </a>
      </nav>
    </header>

    <div class="container-app relative min-h-[677px] flex flex-col items-center pt-8 sm:pt-36">
      <div class="mx-auto mt-[10px] max-w-[1080px] text-center">
        <h1
          class="text-4xl font-semibold leading-tight sm:text-[48px] lg:text-[60px] lg:max-w-[755px]"
        >
          Tenderbot.kz - тендеры и госзакупки Казахстана в одном окне
        </h1>

        <p class="mx-auto text-[24px] mt-8 max-w-[640px] text-white/85">
          Находите подходящие тендеры без ручного поиска.
        </p>

        <div class="mx-auto mt-8 flex justify-center">
          <button
            type="button"
            class="mx-auto inline-flex min-w-[312px] h-[80px] items-center justify-center rounded-full bg-white px-6 py-2 text-[24px] font-bold text-slate-900 shadow-lg shadow-black/10 transition hover:-translate-y-[1px] hover:bg-white/95 active:translate-y-0"
            @click="emit('open-lead')"
          >
            Узнать больше
          </button>
        </div>
      </div>
    </div>
  </section>
</template>
