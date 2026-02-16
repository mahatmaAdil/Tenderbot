<script setup>
import { computed, onBeforeUnmount, ref, watch } from 'vue'

const props = defineProps({
  open: { type: Boolean, default: false },
})
const emit = defineEmits(['close', 'success'])

const close = () => emit('close')

const name = ref('')
const phone = ref('')
const loading = ref(false)
const error = ref('')
const sent = ref(false)

const utmSource = computed(() => {
  try {
    const url = new URL(window.location.href)
    return url.searchParams.get('utm_source') || ''
  } catch {
    return ''
  }
})

function onKey(e) {
  if (e.key === 'Escape') close()
}

watch(
  () => props.open,
  (v) => {
    if (v) {
      document.addEventListener('keydown', onKey)
      document.body.style.overflow = 'hidden'
      error.value = ''
      sent.value = false
      loading.value = false
    } else {
      document.removeEventListener('keydown', onKey)
      document.body.style.overflow = ''
    }
  },
)

onBeforeUnmount(() => {
  document.removeEventListener('keydown', onKey)
  document.body.style.overflow = ''
})

function normalizePhone(p) {
  return String(p || '').trim()
}

async function submit() {
  error.value = ''
  sent.value = false

  const payload = {
    name: String(name.value || '').trim(),
    phone: normalizePhone(phone.value),
    source_id: null,
    utm_source: utmSource.value,
  }

  if (!payload.name) {
    error.value = 'Введите имя'
    return
  }

  if (!payload.phone) {
    error.value = 'Введите телефон'
    return
  }
  if (window.location.host === 'zakup.com.kz') {
    payload.source_id = 1
  }

  if (window.location.host === 'goszakup.com.kz') {
    payload.source_id = 2
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

    sent.value = true
    emit('success', data)

    name.value = ''
    phone.value = ''
    close()
  } catch (e) {
    error.value = e?.message || 'Не удалось отправить заявку'
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <div v-if="open" class="fixed inset-0 z-50">
    <div class="absolute inset-0 bg-black/60" @click="close"></div>

    <div
      class="relative mx-auto mt-16 w-[min(560px,calc(100%-32px))] rounded-2xl bg-white p-5 shadow-2xl"
    >
      <button
        id="modalBtn"
        type="button"
        class="absolute right-3 top-3 grid h-9 w-9 place-items-center rounded-xl text-#078EE6 hover:bg-slate-100"
        @click="close"
        aria-label="Закрыть"
      >
        ✕
      </button>

      <h2 class="text-xl font-extrabold text-slate-900">Заказать звонок / демо</h2>
      <p class="mt-2 text-sm text-slate-600">
        Оставьте контакты — мы свяжемся с вами и покажем сервис.
      </p>

      <form class="mt-4 grid gap-3" @submit.prevent="submit">
        <label class="grid gap-1">
          <span class="text-xs font-semibold text-slate-600">Имя</span>
          <input
            v-model="name"
            class="h-11 rounded-xl border border-slate-200 px-3 outline-none focus:border-blue-400 focus:ring-4 focus:ring-blue-100"
            required
            autocomplete="name"
          />
        </label>

        <label class="grid gap-1">
          <span class="text-xs font-semibold text-slate-600">Телефон</span>
          <input
            v-model="phone"
            class="h-11 rounded-xl border border-slate-200 px-3 outline-none focus:border-blue-400 focus:ring-4 focus:ring-blue-100"
            type="tel"
            required
            autocomplete="tel"
          />
        </label>

        <div v-if="error" class="text-sm font-medium text-red-600">
          {{ error }}
        </div>

        <button class="btn-primary mt-1" id="modalBtn" type="submit" :disabled="loading">
          {{ loading ? 'Отправляю…' : 'Отправить' }}
        </button>

        <p class="text-[11px] text-slate-500">
          Нажимая «Отправить», вы соглашаетесь на обработку данных.
        </p>
      </form>
    </div>
  </div>
</template>
