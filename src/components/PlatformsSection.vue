<script setup>
import KzMap from './KzMap.vue'

import logo1 from '../assets/platforms/logo1.svg'
import logo2 from '../assets/platforms/logo2.svg'
import samruk from '../assets/platforms/samruk.svg'
import logo3 from '../assets/platforms/logo3.svg'

const platforms = [
  { src: logo1, alt: 'Госзакупки' },
  { src: logo2, alt: 'Omarket' },
  { src: samruk, alt: 'Samruk Kazyna' },
  { src: logo3, alt: 'Mitwork' },
]
</script>

<template>
  <section class="bg-white">
    <div class="container-app py-10 sm:py-12">
      <!-- Плашка с текстом и логотипами -->
      <div class="rounded-2xl bg-slate-50 px-6 py-5">
        <div class="flex flex-col gap-4 sm:flex-row sm:items-center sm:justify-between">
          <p class="text-sm text-slate-600 max-w-[520px]">
            Сервис автоматически подберет по вашим параметрам на 142 площадках
            <br />
            <b class="text-slate-900"></b>
          </p>

          <div class="flex flex-wrap items-center gap-6">
            <img
              v-for="p in platforms"
              :key="p.alt"
              :src="p.src"
              :alt="p.alt"
              class="h-8 opacity-60 grayscale transition hover:opacity-100 hover:grayscale-0"
            />
          </div>
        </div>
      </div>

      <!-- Карта -->
      <div class="mt-10">
        <KzMap />
      </div>
    </div>
  </section>
</template>
