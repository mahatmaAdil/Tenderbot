<script setup>
import { computed, ref } from 'vue'
import KzMap from './KzMap.vue'
import { regions } from '../data/regions'

const emit = defineEmits(['open-lead'])

const selectedId = ref('')

const selectedRegion = computed(() => {
  if (!selectedId.value) return null // <-- важно: без выбора возвращаем null
  return regions.find((r) => r.id === selectedId.value) || null
})

function onSelect(id) {
  selectedId.value = id
}
function clearSelection() {
  selectedId.value = ''
}
</script>

<template>
  <section
    class="relative overflow-hidden bg-gradient-to-br from-sky-500 via-blue-600 to-indigo-700 text-white"
  >
    <!-- декор -->
    <div
      class="pointer-events-none absolute -top-28 -left-28 h-80 w-80 rounded-full bg-white/15 blur-3xl"
    ></div>
    <div
      class="pointer-events-none absolute -bottom-28 -right-28 h-80 w-80 rounded-full bg-white/10 blur-3xl"
    ></div>

    <!-- сетка линий -->
    <div class="pointer-events-none absolute inset-0 opacity-25">
      <div
        class="h-full w-full bg-[linear-gradient(to_right,rgba(255,255,255,.25)_1px,transparent_1px),linear-gradient(to_bottom,rgba(255,255,255,.25)_1px,transparent_1px)] bg-[size:28px_28px]"
      ></div>
    </div>

    <div class="container-app relative py-14 sm:py-16 lg:py-20">
      <div class="grid items-center gap-10 lg:grid-cols-[1.1fr_0.9fr]">
        <!-- Текст -->
        <div>
          <p class="text-sm font-semibold tracking-wide text-white/85">Tenderbot.kz</p>

          <h1 class="mt-3 text-3xl font-extrabold leading-tight sm:text-4xl lg:text-5xl">
            Tenderbot.kz – тендеры и госзакупки Казахстана в одном окне
          </h1>

          <p class="mt-4 max-w-xl text-base text-white/85 sm:text-lg">
            Находите подходящие тендеры без ручного поиска. Сервис автоматически подберет по вашим
            параметрам на 142 площадках.
          </p>

          <div class="mt-7 flex flex-col gap-3 sm:flex-row sm:items-center">
            <button class="btn-primary" type="button" @click="emit('open-lead')">
              Узнать больше
            </button>
            <a class="btn-ghost" href="#features">Смотреть возможности</a>
          </div>

          <div class="mt-8 grid max-w-xl grid-cols-2 gap-3 sm:grid-cols-3">
            <div class="rounded-2xl bg-white/10 px-4 py-3">
              <div class="text-lg font-extrabold">142</div>
              <div class="text-xs text-white/80">площадки</div>
            </div>
            <div class="rounded-2xl bg-white/10 px-4 py-3">
              <div class="text-lg font-extrabold">24/7</div>
              <div class="text-xs text-white/80">мониторинг</div>
            </div>
            <div class="rounded-2xl bg-white/10 px-4 py-3">
              <div class="text-lg font-extrabold">30 мин</div>
              <div class="text-xs text-white/80">обновления</div>
            </div>
          </div>
        </div>

        <!-- Правая карточка (место под карту) -->
        <div class="relative">
          <div class="card p-4 sm:p-6">
            <div class="flex items-center justify-between">
              <div>
                <div class="text-sm font-semibold text-slate-900">Карта Казахстана</div>
                <div class="text-xs text-slate-500">
                  кликабельные регионы — сделаем следующим шагом
                </div>
              </div>
              <div class="rounded-xl bg-blue-50 px-3 py-1 text-xs font-semibold text-blue-700">
                Demo
              </div>
            </div>

            <div class="mt-4 relative">
              <KzMap v-model="selectedId" @select="onSelect" />

              <!-- ACTIVE OVERLAY (как в фигме) -->
              <div
                v-if="selectedRegion"
                class="absolute left-4 top-4 w-[min(320px,calc(100%-32px))] rounded-2xl bg-white p-4 shadow-2xl ring-1 ring-black/10"
              >
                <div class="flex items-start justify-between gap-3">
                  <div>
                    <div class="text-xs font-semibold text-slate-500">Вы выбрали регион</div>
                    <div class="mt-1 text-base font-extrabold text-slate-900">
                      {{ selectedRegion.name }}
                    </div>
                  </div>

                  <button
                    type="button"
                    class="grid h-9 w-9 place-items-center rounded-xl text-slate-500 hover:bg-slate-100"
                    @click="clearSelection"
                    aria-label="Закрыть"
                  >
                    ✕
                  </button>
                </div>

                <div class="mt-3 grid grid-cols-2 gap-2">
                  <div class="rounded-xl bg-slate-50 px-3 py-2">
                    <div class="text-xs text-slate-500">Тендеров</div>
                    <div class="text-sm font-bold text-slate-900">
                      {{ selectedRegion.tenders }}
                    </div>
                  </div>

                  <div class="rounded-xl bg-slate-50 px-3 py-2">
                    <div class="text-xs text-slate-500">Частота</div>
                    <div class="text-sm font-bold text-slate-900">каждые 30 мин</div>
                  </div>
                </div>

                <button type="button" class="btn-primary mt-4 w-full" @click="emit('open-lead')">
                  Записаться на демо
                </button>

                <p class="mt-2 text-[11px] text-slate-500">
                  Покажем крупные тендеры и настроим подборку под ваш бизнес.
                </p>
              </div>
            </div>

            <div class="mt-4 grid grid-cols-3 gap-2">
              <div class="rounded-xl bg-slate-50 px-3 py-2">
                <div class="text-xs text-slate-500">Тендеров</div>
                <div class="text-sm font-bold text-slate-900">1000+</div>
              </div>
              <div class="rounded-xl bg-slate-50 px-3 py-2">
                <div class="text-xs text-slate-500">Фильтров</div>
                <div class="text-sm font-bold text-slate-900">16</div>
              </div>
              <div class="rounded-xl bg-slate-50 px-3 py-2">
                <div class="text-xs text-slate-500">Рассылки</div>
                <div class="text-sm font-bold text-slate-900">∞</div>
              </div>
            </div>
          </div>

          <div class="pointer-events-none absolute -inset-6 -z-10 opacity-35">
            <div class="h-full w-full rounded-[28px] bg-white/10 blur-2xl"></div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
