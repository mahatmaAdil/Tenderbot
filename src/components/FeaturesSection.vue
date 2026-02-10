<script setup>
import feature1 from '@/assets/features/Features1.svg'
import feature2 from '@/assets/features/Features2.svg'
import feature3 from '@/assets/features/Features3.svg'
import feature4 from '@/assets/features/Features4.svg'

const features = [
  {
    icon: feature1,
    title: '142 тендерные площадки',
    text: 'Вся база госзакупок и коммерческих тендеров Казахстана: Omarket, Госзакупки, Самрук Казына, Tizilim, Mitwork и другие.',
  },
  {
    icon: feature2,
    title: 'Регулярная подборка тендеров',
    text: 'Автоматизированные email-рассылки и персональные подборки без ограничений. Умный поиск по 16 фильтрам.',
  },
  {
    icon: feature3,
    title: 'Сопровождение и обучение',
    text: 'Помощь в участии в госзакупках и коммерческих тендерах, подготовка заявок и поддержка практикующих закупщиков.',
  },
  {
    icon: feature4,
    title: 'Сделано для бизнеса',
    text: 'Разовые подачи, гибкие тарифы, поддержка через мессенджеры. Платформа ищет тендеры для вас 24/7.',
  },
]
</script>

<template>
  <section class="-mt-10">
    <div class="container-app py-14 sm:py-18">
      <div class="mt-10 grid gap-4 sm:mt-12 sm:grid-cols-2">
        <div v-for="f in features" :key="f.title" class="rounded-2xl border bg-white p-6">
          <div
            class="mb-4 flex h-14 w-14 items-center justify-center rounded-xl bg-gradient-to-b from-[#00BCFF] to-[#155DFC]"
          >
            <img :src="f.icon" alt="" class="h-7 w-7" />
          </div>

          <h3 class="text-base font-bold text-slate-900">
            {{ f.title }}
          </h3>

          <p class="mt-2 text-sm text-slate-600">
            {{ f.text }}
          </p>
        </div>
      </div>
    </div>
  </section>
</template>
