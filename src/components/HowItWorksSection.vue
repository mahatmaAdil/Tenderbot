<script setup>
const steps = [
  {
    n: '01',
    title: 'Вы задаёте параметры',
    text: 'Отрасль, регион, сумма, заказчик, ключевые слова.',
  },
  {
    n: '02',
    title: 'Система ищет за вас',
    text: 'Tenderbot круглосуточно отслеживает новые тендеры на всех площадках.',
  },
  {
    n: '03',
    title: 'Вы получаете результат',
    text: 'Подходящие тендеры приходят на email и в ваш личный кабинет каждые полчаса.',
  },
]
</script>

<template>
  <!-- В Фигме этот блок на светло-сером фоне -->
  <section class="bg-slate-50">
    <div class="container-app py-14 sm:py-16">
      <h2 class="text-center text-2xl font-extrabold text-slate-900 sm:text-3xl">
        Как работает сервис?
      </h2>

      <div class="mt-10 grid gap-6 sm:grid-cols-3">
        <div
          v-for="s in steps"
          :key="s.n"
          class="rounded-2xl bg-white px-6 py-6 shadow-[0_10px_30px_rgba(15,23,42,0.06)] ring-1 ring-black/5"
        >
          <div class="flex items-start gap-4">
            <!-- круглый бейдж как в макете -->
            <div
              class="grid h-12 w-12 shrink-0 place-items-center rounded-full bg-blue-100 text-sm font-extrabold text-blue-700"
            >
              {{ s.n }}
            </div>

            <div>
              <div class="text-sm font-extrabold text-slate-900">
                {{ s.title }}
              </div>

              <p class="mt-2 text-xs leading-relaxed text-slate-500">
                {{ s.text }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
