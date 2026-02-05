<script setup>
import KzMap from './KzMap.vue'

import logo1 from '../assets/platforms/logo1.svg'
import logo2 from '../assets/platforms/logo2.svg'
import samruk from '../assets/platforms/samruk.svg'
import logo3 from '../assets/platforms/logo3.svg'
</script>

<template>
  <section class="">
    <div class="container-app py-10 sm:py-12">
      <div class="rounded-2xl bg-slate-50 px-6 py-5">
        <div class="flex flex-col h-auto gap-4 sm:flex-row sm:items-center sm:justify-between">
          <p class="max-w-[330px] text-sm text-slate-600">
            Сервис автоматически подберет по вашим параметрам на 142 площадках
            <br />
            <b class="text-slate-900"></b>
          </p>

          <div class="grid grid-cols-2 gap-x-8 gap-y-4 sm:flex sm:items-center sm:gap-8">
            <div class="flex h-12 items-center justify-center">
              <img :src="logo2" class="h-full w-auto object-contain" />
            </div>

            <div class="flex h-14 items-center justify-center">
              <img :src="samruk" class="h-full w-auto object-contain" />
            </div>

            <div class="flex h-8 items-center justify-center">
              <img :src="logo1" class="h-full w-auto object-contain" />
            </div>

            <div class="flex h-8 items-center justify-center">
              <img :src="logo3" class="h-full w-auto object-contain" />
            </div>
          </div>
        </div>
      </div>

      <div class="mt-10">
        <KzMap />
      </div>
    </div>
  </section>
</template>
