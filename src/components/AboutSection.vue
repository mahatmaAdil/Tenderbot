<script setup>
import { ref } from 'vue'
import tenderbotIcon from '../assets/icons/tenderbot.svg'
import icon1 from '../assets/icons/icon1.svg'
import icon2 from '../assets/icons/icon2.svg'
import icon3 from '../assets/icons/icon3.svg'
const country = ref('Казахстан')
const name = ref('')
const phone = ref('')
function onSubmit() {
  console.log({ country: country.value, name: name.value, phone: phone.value })
}
</script>
<template>
  <section class="">
    <div class="container-app py-14 sm:py-18">
      <h2 class="text-center text-2xl font-extrabold text-slate-900 sm:text-3xl">
        Tenderbot — это не только поиск закупок
      </h2>
      <div class="mt-10 grid gap-6 sm:grid-cols-3">
        <div class="rounded-2xl border border-slate-200 bg-white p-6">
          <div class="mb-4"><img :src="icon1" alt="" class="h-8 w-8" /></div>
          <div class="text-sm font-extrabold text-slate-900">Обучаем тендерам онлайн и оффлайн</div>
          <p class="mt-2 text-sm text-slate-600">70% практических кейсов от реальных закупщиков.</p>
        </div>
        <div class="rounded-2xl border border-slate-200 bg-white p-6">
          <div class="mb-4"><img :src="icon2" alt="" class="h-8 w-8" /></div>
          <div class="text-sm font-extrabold text-slate-900">Сопровождаем в участии</div>
          <p class="mt-2 text-sm text-slate-600">
            Гарантируем 100% подачу на тендер, обеспечиваем юридическую и финансовую поддержку.
          </p>
        </div>
        <div class="rounded-2xl border border-slate-200 bg-white p-6">
          <div class="mb-4"><img :src="icon3" alt="" class="h-8 w-8" /></div>
          <div class="text-sm font-extrabold text-slate-900">
            Предоставляем аналитику госзакупок
          </div>
          <p class="mt-2 text-sm text-slate-600">
            Полная аналитика: данные по заказчикам, суммам и направлениям на 2026 год.
          </p>
        </div>
      </div>
      <div class="mt-10 rounded-3xl bg-white p-6 sm:p-10">
        <div class="grid gap-8 lg:grid-cols-2 lg:items-center">
          <div>
            <img
              :src="tenderbotIcon"
              alt="Tenderbot"
              class="mb-6 -mt-10 -translate-y-3 h-8 w-auto select-none"
              draggable="false"
            />
            <div class="text-xl font-extrabold text-slate-900 sm:text-5xl">
              Всё ещё думаете?<br />
              Просто попробуйте
            </div>
            <p class="mt-4 max-w-[520px] text-[17px] leading-relaxed text-slate-600">
              Если сомневаетесь или что-то не поняли — смело оставляйте свои контакты. Наш менеджер
              перезвонит и поможет начать работать в сервисе.
            </p>
          </div>
          <div class="rounded-2xl border border-slate-200 bg-white p-5 sm:p-6">
            <form class="grid gap-3" @submit.prevent="onSubmit">
              <label class="grid gap-1">
                <span class="text-xs font-semibold text-slate-600">Страна</span>
                <select
                  v-model="country"
                  class="h-11 rounded-xl border border-slate-200 px-3 text-sm outline-none focus:border-blue-400 focus:ring-4 focus:ring-blue-100"
                >
                  <option>Казахстан</option>
                  <option>Кыргызстан</option>
                  <option>Узбекистан</option>
                </select>
              </label>
              <label class="grid gap-1">
                <span class="text-xs font-semibold text-slate-600">Имя</span>
                <input
                  v-model="name"
                  class="h-11 rounded-xl border border-slate-200 px-3 text-sm outline-none focus:border-blue-400 focus:ring-4 focus:ring-blue-100"
                  placeholder="Имя"
                  required
                />
              </label>
              <label class="grid gap-1">
                <span class="text-xs font-semibold text-slate-600">Телефон</span>
                <input
                  v-model="phone"
                  class="h-11 rounded-xl border border-slate-200 px-3 text-sm outline-none focus:border-blue-400 focus:ring-4 focus:ring-blue-100"
                  placeholder="Телефон"
                  type="tel"
                  required
                />
              </label>
              <button
                type="submit"
                class="mt-2 h-11 rounded-xl bg-blue-600 text-sm font-semibold text-white transition hover:bg-blue-500"
              >
                Отправить заявку
              </button>
              <p class="text-[11px] leading-snug text-slate-500">
                Заполняя форму, вы подтверждаете согласие на обработку персональных данных.
              </p>
            </form>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
