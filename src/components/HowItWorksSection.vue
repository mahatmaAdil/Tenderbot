<script setup>
const steps = [
  {
    n: '01',
    title: 'Вы задаёте параметры',
    text: 'Отрасль, регион, сумма, заказчик, ключевые слова. ',
  },
  {
    n: '02',
    title: 'Система ищет за вас',
    text: 'Tenderbot круглосуточно отслеживает новые тендеры на всех площадках.',
  },
  {
    n: '03',
    title: 'Вы получаете результат',
    text: 'Подходящие тендеры приходят на email и в ваш личный кабинет.',
  },
]
</script>

<template>
  <section id="how">
    <div class="container-app py-14 sm:py-16">
      <h2 class="text-center text-2xl font-semibold text-slate-900 sm:text-3xl">
        Как работает сервис?
      </h2>

      <div class="mt-10 grid gap-6 sm:grid-cols-3">
        <div v-for="s in steps" :key="s.n" class="card-step">
          <div class="pointer-events-none absolute inset-x-0 bottom-[7px] h-[7px]"></div>

          <div class="flex items-center gap-4">
            <div
              class="grid h-16 w-16 shrink-0 place-items-center rounded-full bg-[#D3E8F6] text-[25px] font-bold text-[#078EE6]"
            >
              {{ s.n }}
            </div>

            <div>
              <div class="text-base font-semibold text-slate-900 mb-2">
                {{ s.title }}
              </div>

              <p
                class="text-sm leading-tight text-slate-500 font-Inter"
                :class="{ 'step-text-fix': s.n === '01' }"
              >
                {{ s.text }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.step-text-fix::after {
  content: 'xxxxxxxxxxxxxxxxxxxx';
  opacity: 0;
  pointer-events: none;
  user-select: none;
}
.card-step {
  position: relative;
  overflow: hidden;
  border-radius: 16px;
  background: white;
  padding: 24px;
  box-shadow: 0 12px 32px rgba(15, 23, 42, 0.08);
}

.card-step::after {
  content: '';
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  height: 7px;
  background: linear-gradient(90deg, #088ee600 10%, #078ee5 50%, #098fe500 90%);
  mask-image: linear-gradient(to top, rgba(0, 0, 0, 1) 60%, rgba(0, 0, 0, 0));
}
</style>
