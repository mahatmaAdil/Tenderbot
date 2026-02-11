<script setup>
import KzMap from '@/components/KzMap.vue'
import logo1 from '@/assets/platforms/logo1.svg'
import logo2 from '@/assets/platforms/logo2.svg'
import samruk from '@/assets/platforms/samruk.svg'
import logo3 from '@/assets/platforms/logo3.svg'
</script>

<template>
  <section id="tenders" class="relative z-20 scroll-mt-28">
    <div class="container-app pb-5 sm:pb-10 -mt-[120px] sm:-mt-12 relative z-30">
      <div class="relative rounded-2xl bg-slate-50 px-6 py-5">
        <div class="flex flex-col h-auto gap-4 sm:flex-row sm:items-center sm:justify-between">
          <p class="max-w-[330px] text-sm text-slate-600">
            <span class="hidden sm:inline">
              Сервис автоматически подберет по вашим параметрам на 142 площадках
            </span>

            <span class="sm:hidden">
              <b class="text-lg text-slate-900">Полный доступ к базе закупок - 142 площадок</b>
              <br />
              <span class="text-slate-400">
                (включая goszakup.gov.kz, zakup.sk.kz, tizilim.gov.kz, eep.mitwork.kz)
              </span>
            </span>
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
