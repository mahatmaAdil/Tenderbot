<script setup>
import { computed, ref } from 'vue'
import tenderbotIcon from '@/assets/icons/Tenderbot.svg'
import icon1 from '@/assets/icons/icon1.svg'
import icon2 from '@/assets/icons/icon2.svg'
import icon3 from '@/assets/icons/icon3.svg'

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
    <div class="container-app py-14 sm:py-18">
      <h2 class="text-center text-2xl font-bold text-slate-900 sm:text-3xl">
        Tenderbot — это не только поиск закупок
      </h2>

      <div class="mt-10 grid gap-6 sm:grid-cols-3">
        <div class="rounded-2xl border border-slate-200 p-6">
          <div class="mb-4"><img :src="icon1" alt="" class="h-8 w-8" /></div>
          <div class="text-sm font-bold text-slate-900">Обучаем тендерам онлайн и оффлайн</div>
          <p class="mt-2 text-sm text-slate-600">70% практических кейсов от реальных закупщиков.</p>
        </div>

        <div class="rounded-2xl border border-slate-200 p-6">
          <div class="mb-4"><img :src="icon2" alt="" class="h-8 w-8" /></div>
          <div class="text-sm font-bold text-slate-900">Сопровождаем в участии</div>
          <p class="mt-2 text-sm text-slate-600">
            Гарантируем 100% подачу на тендер, обеспечиваем юридическую и финансовую поддержку.
          </p>
        </div>

        <div class="rounded-2xl border border-slate-200 p-6">
          <div class="mb-4"><img :src="icon3" alt="" class="h-8 w-8" /></div>
          <div class="text-sm font-bold text-slate-900">Предоставляем аналитику госзакупок</div>
          <p class="mt-2 text-sm text-slate-600">
            Полная аналитика: данные по заказчикам, суммам и направлениям на 2026 год.
          </p>
        </div>
      </div>

      <div class="mt-10 rounded-3xl bg-white p-6 sm:p-10">
        <div class="grid gap-4 lg:grid-cols-2 lg:items-center">
          <div>
            <img
              :src="tenderbotIcon"
              alt="Tenderbot"
              class="pt-0 sm:pt-10 mt-4 translate-y-0 sm:-mt-10 sm:-translate-y-3 w-auto max-w-full select-none"
              draggable="false"
            />
            <div class="text-2xl max-w-[400px] font-semibold text-slate-900 sm:text-4xl">
              Всё ещё думаете?<br />Просто <br />попробуйте
            </div>
            <p class="mt-4 max-w-[400px] text-[15px] leading-relaxed text-slate-600">
              Если сомневаетесь или что-то не поняли - смело оставляйте свои контакты. Наш менеджер
              перезвонит и поможет начать работать в сервисе.
            </p>
          </div>

          <div id="form" class="rounded-2xl border border-slate-200 bg-white p-5 sm:p-6">
            <form class="grid gap-3" @submit.prevent="onSubmit">
              <label class="relative block">
                <img
                  src="@/assets/icons/icon5.svg"
                  alt=""
                  class="pointer-events-none absolute left-3 top-1/2 h-4 w-4 -translate-y-1/2 opacity-70"
                />
                <input
                  v-model="name"
                  class="h-11 w-full rounded-xl border border-slate-200 pl-10 pr-3 text-sm outline-none focus:border-blue-400 focus:ring-4 focus:ring-blue-100"
                  placeholder="Имя"
                  required
                  :disabled="loading"
                />
              </label>

              <label class="relative block">
                <img
                  src="@/assets/icons/icon6.svg"
                  alt=""
                  class="pointer-events-none absolute left-3 top-1/2 h-4 w-4 -translate-y-1/2 opacity-70"
                />
                <input
                  v-model="phone"
                  type="tel"
                  placeholder="Телефон"
                  required
                  class="h-11 w-full rounded-xl border border-slate-200 pl-10 pr-3 text-sm outline-none focus:border-blue-400 focus:ring-4 focus:ring-blue-100"
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
                type="submit"
                class="mt-2 h-11 rounded-xl bg-[#078EE6] text-sm font-semibold text-white transition hover:bg-blue-400 disabled:opacity-60"
                :disabled="loading"
              >
                {{ loading ? 'Отправляю…' : 'Отправить заявку' }}
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
