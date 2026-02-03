<script setup>
import KzMap from './KzMap.vue'

import logo1 from '../assets/platforms/logo1.svg'
import logo2 from '../assets/platforms/logo2.svg'
import samruk from '../assets/platforms/samruk.svg'
import logo3 from '../assets/platforms/logo3.svg'
</script>

<template>
  <section class="bg-white">
    <div class="container-app py-10 sm:py-12">
      <!-- Плашка с текстом и логотипами -->
      <div class="rounded-2xl bg-slate-50 px-6 py-5">
        <div class="flex flex-col gap-4 sm:flex-row sm:items-center sm:justify-between">
          <p class="max-w-[420px] text-sm text-slate-600">
            Сервис автоматически подберет по вашим параметрам на 142 площадках
            <br />
            <b class="text-slate-900"></b>
          </p>

          <div class="flex flex-wrap items-center gap-6">
            <img :src="logo1" alt="Госзакупки" class="h-8" />
            <img :src="logo2" alt="Omarket" class="h-8" />
            <img :src="samruk" alt="Samruk Kazyna" class="h-8" />
            <img :src="logo3" alt="Mitwork" class="h-8" />
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
