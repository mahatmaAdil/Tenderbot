<script setup>
import { computed, ref } from 'vue'
import tenderbotIcon from '@/assets/icons/Tenderbot.svg'
import icon1 from '@/assets/icons/icon1.svg'
import icon2 from '@/assets/icons/icon2.svg'
import icon3 from '@/assets/icons/icon3.svg'

const METRIKA_ID = 106181212
const GOAL_ID = 'submit_form_send_a_request'

const name = ref('')
const phone = ref('')

const loading = ref(false)
const error = ref('')
const success = ref(false)

const utmSource = computed(() => {
  if (typeof window === 'undefined') return ''
  return new URLSearchParams(window.location.search).get('utm_source') || ''
})

function normalizePhone(p) {
  return String(p || '')
    .replace(/[^\d+]/g, '')
    .trim()
}

async function onSubmit() {
  error.value = ''
  success.value = false

  const payload = {
    name: String(name.value || '').trim(),
    phone: normalizePhone(phone.value),
    utm_source: utmSource.value,
  }

  if (!payload.name) {
    error.value = 'Введите имя'
    return
  }

  const digits = payload.phone.replace(/\D/g, '')
  if (!digits || digits.length < 10) {
    error.value = 'Введите корректный телефон'
    return
  }

  loading.value = true
  try {
    const res = await fetch('https://api.tenderbot.kz/api/promo/ask-call', {
      method: 'POST',
      headers: {
        Accept: 'application/json',
        'Content-Type': 'application/json',
        Authorization:
          'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6IjY2MTNiMDZiZTIyOWYxNDM4NTcxOTdhZmExODhjZTk1NDYyMDRlNWVmOWU0ZGI3NDg4OTJhMTJlZGI2NTJlMTI4OGQ2MWUxZmY0MDZjZGIxIn0.eyJhdWQiOiIxIiwianRpIjoiNjYxM2IwNmJlMjI5ZjE0Mzg1NzE5N2FmYTE4OGNlOTU0NjIwNGU1ZWY5ZTRkYjc0ODg5MmExMmVkYjY1MmUxMjg4ZDYxZTFmZjQwNmNkYjEiLCJpYXQiOjE3NzAzODExMTMsIm5iZiI6MTc3MDM4MTExMywiZXhwIjoxODAxOTE3MTEzLCJzdWIiOiI2Mjg0NiIsInNjb3BlcyI6W119.oGyWJq8HTSeXprjrJWDnoI1TaNcXKJt12asm3wJBtfMEcCvkWPfbh61JeWqLwUX8JP_I2AuILNw9I-xGvaqNAZyO43odolzgDm6RzCuQstfESPuSk7J70l4juf49IiG88V_jX7xrdLOcXSwhsRD6ov9DgpuZQ91hfipYITa_oB0e6t-VjKCDWiJG5WyTbWno_l172CaGk9iGBHrTFAm13OFfNlimY4aTzgnAuFLFIzsUlkRHhDO22CrPlnpyYrELe498AFUAPsWXi4lj3xiPdM8iK7wdcGHLWkXz5mkbrS3z1nXgXbjRKirBmSsNFYuM9IIHdEmC8fZYQLieFO4fmNBedX4gmQo59eLtrwiQLEi3tiB9u5j9KAu3CnfQmE1TItyft-X8byBOi7_PSIwECmm4w_Je229i8hGKNa1jxbJVOD6BCoQFI0JjZsRNqcXv_-WLAMkrivZsMVIZ1I23uWuPslAbqQ8xjPkLnpTd-Qf5VkN068nCrWgEpyodVfjuVRRBmPSIwXLE96I5rF98WJcxI7WBBkEA8nbQ55Ez4pohICbs4McA7gpHz2WoF4NGbrLjcGYlhtucgwr4Dvqd_Hz94QErlc21laLDPA8vADIK13yYfMePilr4cF6pUXKUJq2lMnum0_y7mK98QEASgq7fcHCAqzeYE8IVcqA8Qno',
      },
      body: JSON.stringify(payload),
    })

    const text = await res.text()
    let data = null
    try {
      data = text ? JSON.parse(text) : null
    } catch {
      data = text || null
    }

    if (!res.ok) {
      const msg = (data && (data.message || data.error)) || `Ошибка отправки (HTTP ${res.status})`
      throw new Error(msg)
    }
    if (typeof window !== 'undefined' && typeof window.ym === 'function') {
      console.log('GOAL FIRED:', GOAL_ID)
      window.ym(METRIKA_ID, 'reachGoal', GOAL_ID)
    }

    success.value = true
    name.value = ''
    phone.value = ''
    setTimeout(() => (success.value = false), 2500)
  } catch (e) {
    error.value = e?.message || 'Не удалось отправить заявку'
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <section id="about">
    <div class="container-app py-14 sm:py-18 mx-auto px-4 sm:px-6">
      <h2 class="text-center text-2xl font-semibold text-slate-900 sm:text-3xl">
        Tenderbot — это не только поиск закупок
      </h2>

      <div class="mt-10 grid gap-6 sm:grid-cols-3">
        <div class="rounded-2xl border border-slate-200 p-4">
          <div class="mb-4"><img :src="icon1" alt="" class="h-8 w-8" /></div>
          <div class="text-base font-semibold text-slate-900">
            Обучаем тендерам онлайн и оффлайн
          </div>
          <p class="mt-2 text-sm text-slate-600">70% практических кейсов от реальных закупщиков.</p>
        </div>

        <div class="rounded-2xl border border-slate-200 p-4">
          <div class="mb-4"><img :src="icon2" alt="" class="h-8 w-8" /></div>
          <div class="text-base font-bold text-slate-900">Сопровождаем в участии</div>
          <p class="mt-2 text-sm text-slate-600">
            Гарантируем 100% подачу на тендер, обеспечиваем юридическую и финансовую поддержку.
          </p>
        </div>

        <div class="rounded-2xl border border-slate-200 p-4">
          <div class="mb-4"><img :src="icon3" alt="" class="h-8 w-8" /></div>
          <div class="text-base font-bold text-slate-900 whitespace-nowrap">
            Предоставляем аналитику госзакупок
          </div>
          <p class="mt-2 text-sm text-slate-600">
            Предоставляем полную аналитику госзакупок. Данные по заказчикам, суммам и направлениям
            на 2026 год.
          </p>
        </div>
      </div>

      <div id="form" class="mt-16 rounded-3xl bg-[#FFF] p-5 sm:p-10">
        <div class="grid gap-10 lg:grid-cols-2 lg:items-center">
          <div class="mx-auto max-[600px] p-6 sm:p-10">
            <img
              :src="tenderbotIcon"
              alt="Tenderbot"
              class="h-8 w-auto select-none"
              draggable="false"
            />

            <h3 class="mt-8 text-3xl font-semibold leading-tight text-slate-900 sm:text-[36px]">
              Всё еще думаете? <br />
              Просто попробуйте
            </h3>

            <p class="mt-6 text-base sm:text-xl leading-tight text-slate-900">
              Если сомневаетесь или что-то не поняли, смело оставляйте свои контакты — наш менеджер
              перезвонит и поможет начать работать в сервисе.
            </p>
          </div>

          <div
            class="mx-auto w-full max-w-[600px] rounded-3xl border border-slate-200 bg-white p-6 sm:p-10"
          >
            <form class="grid gap-6" @submit.prevent="onSubmit">
              <label class="relative block">
                <img
                  src="@/assets/icons/icon5.svg"
                  alt=""
                  class="pointer-events-none absolute left-0 top-1/2 h-4 w-4 -translate-y-1/2 opacity-60"
                />
                <input
                  v-model="name"
                  class="h-12 w-full border-b border-slate-200 bg-transparent pl-7 pr-2 text-sm text-slate-900 outline-none placeholder:text-slate-400 focus:border-blue-500"
                  placeholder="Имя"
                  required
                  :disabled="loading"
                />
              </label>

              <label class="relative block">
                <img
                  src="@/assets/icons/icon6.svg"
                  alt=""
                  class="pointer-events-none absolute left-0 top-1/2 h-4 w-4 -translate-y-1/2 opacity-60"
                />
                <input
                  v-model="phone"
                  type="tel"
                  class="h-12 w-full border-b border-slate-200 bg-transparent pl-7 pr-2 text-sm text-slate-900 outline-none placeholder:text-slate-400 focus:border-blue-500"
                  placeholder="Телефон"
                  required
                  :disabled="loading"
                />
              </label>

              <div v-if="error" class="text-sm font-semibold text-red-600">
                {{ error }}
              </div>

              <div v-if="success" class="text-sm font-semibold text-emerald-600">
                Заявка отправлена
              </div>

              <button
                id="formBtn"
                type="submit"
                class="mx-auto mt-2 h-10 w-[240px] rounded-lg bg-[#078EE6] text-sm font-semibold text-white transition hover:bg-blue-500 disabled:opacity-60"
                :disabled="loading"
              >
                {{ loading ? 'Отправляю…' : 'Отправить заявку' }}
              </button>

              <p class="mx-auto max-w-[280px] text-center text-[10px] leading-snug text-slate-400">
                Заполняя форму, вы подтверждаете согласие на обработку персональных данных.
              </p>
            </form>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
