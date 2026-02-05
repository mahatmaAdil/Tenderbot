<script setup>
import { computed, nextTick, onMounted, onUnmounted, ref, watch } from 'vue'
import mapSvgRaw from '../assets/map.svg?raw'
import { regions } from '../data/regions'

const props = defineProps({
  modelValue: { type: String, default: '' },
})

const emit = defineEmits(['update:modelValue', 'select'])

const host = ref(null) // контейнер с v-html (svg внутри)
const desktopTip = ref(null)
const mobileTip = ref(null)

const activeId = computed(() => props.modelValue || '')

const tooltip = ref({
  visible: false,
  x: 0,
  y: 0,
  region: null,
})

const regionMap = computed(() => {
  const m = new Map()
  regions.forEach((r) => m.set(r.id, r))
  return m
})

const highlightedId = computed(() => {
  // пока тултип открыт — подсвечиваем его регион
  if (tooltip.value.visible) return tooltip.value.region?.id || ''
  // иначе (если надо) можно подсвечивать выбранное снаружи
  return props.modelValue || ''
})

function setActive(id, name, tenders) {
  emit('update:modelValue', id)
  emit('select', { id, name, tenders })
}

function applyActiveClass() {
  const root = host.value
  if (!root) return

  root.querySelectorAll('path[id]').forEach((p) => {
    p.classList.toggle('is-active', p.id === highlightedId.value)
  })
}

watch(highlightedId, applyActiveClass, { immediate: true })

function closeTooltip() {
  tooltip.value.visible = false
  tooltip.value.region = null
  applyActiveClass()
}

function openTooltipDesktop(e, region) {
  const container = host.value
  const rect = container.getBoundingClientRect()

  tooltip.value.visible = true
  tooltip.value.region = region
  tooltip.value.x = e.clientX - rect.left + 12
  tooltip.value.y = e.clientY - rect.top + 12
}

// ====== adaptive ======
const isMobile = ref(false)

function updateIsMobile() {
  isMobile.value = window.innerWidth < 768
}

// закрытие по тапу/клику вне карты и вне тултипа (и на мобиле, и на десктопе)
function onDocPointerDown(e) {
  if (!tooltip.value.visible) return

  const t = e.target

  // клик по региону (path) — НЕ закрываем
  if (t.closest('path[id]')) return

  // клик по тултипу — НЕ закрываем
  if (desktopTip.value?.contains?.(t)) return
  if (mobileTip.value?.contains?.(t)) return

  // всё остальное — закрываем
  closeTooltip()
}

onMounted(async () => {
  updateIsMobile()
  window.addEventListener('resize', updateIsMobile)
  document.addEventListener('pointerdown', onDocPointerDown)

  await nextTick()

  const root = host.value
  if (!root) return

  // подтюним svg, чтобы был адаптивный
  const svg = root.querySelector('svg')
  if (svg) {
    svg.setAttribute('preserveAspectRatio', 'xMidYMid meet')

    if (!svg.getAttribute('viewBox')) {
      const w = parseFloat(svg.getAttribute('width') || '0')
      const h = parseFloat(svg.getAttribute('height') || '0')
      if (w > 0 && h > 0) svg.setAttribute('viewBox', `0 0 ${w} ${h}`)
    }
  }

  const paths = root.querySelectorAll('path[id]')

  paths.forEach((p) => {
    p.classList.add('region')

    const id = p.id
    const r = regionMap.value.get(id)

    const region = {
      id,
      name: r?.name || id,
      tenders: Number(r?.tenders ?? 0),
    }

    // ✅ и на десктопе, и на мобиле: всё по клику
    p.addEventListener('click', (e) => {
      e.stopPropagation()

      setActive(region.id, region.name, region.tenders)

      // toggle по тому же региону
      if (tooltip.value.visible && tooltip.value.region?.id === region.id) {
        closeTooltip()
        return
      }

      if (isMobile.value) {
        tooltip.value.visible = true
        tooltip.value.region = region
        return
      }

      openTooltipDesktop(e, region)
    })
  })

  applyActiveClass()
})

onUnmounted(() => {
  window.removeEventListener('resize', updateIsMobile)
  document.removeEventListener('pointerdown', onDocPointerDown)
})
</script>

<template>
  <div class="relative">
    <div class="flex flex-col">
      <div class="flex items-center justify-between">
        <div class="flex flex-col gap-1">
          <div class="max-w-[240px] text-xl font-bold leading-tight text-slate-900">
            Карта тендеров в Казахстане
          </div>

          <div class="max-w-[240px] text-sm leading-snug text-slate-500">
            Нажмите на область, чтобы узнать о проводимых тендерах
          </div>
        </div>

        <div
          v-if="activeId"
          class="rounded-xl bg-blue-50 px-3 py-1 text-xs font-semibold text-blue-700"
        >
          Выбрано:
          {{ regionMap.get(activeId)?.name || activeId }}
        </div>
      </div>

      <div ref="host" class="w-full" v-html="mapSvgRaw"></div>

      <div
        v-if="tooltip.visible && !isMobile"
        ref="desktopTip"
        class="absolute z-50 min-w-[180px] rounded-xl bg-white px-3 py-3 text-xs shadow-xl ring-1 ring-black/10"
        :style="{ left: tooltip.x + 'px', top: tooltip.y + 'px' }"
        @pointerdown.stop
        @click.stop
      >
        <div class="font-bold text-slate-900">
          {{ tooltip.region?.name }}
        </div>

        <div class="mt-1 text-lg font-bold text-blue-700">
          {{ tooltip.region?.tenders }} тендеров
        </div>

        <button
          type="button"
          class="mt-3 inline-flex w-full items-center justify-center rounded-lg bg-blue-600 px-3 py-2 text-xs font-semibold text-white transition hover:bg-blue-700"
        >
          Смотреть
        </button>
      </div>
    </div>

    <div
      v-if="tooltip.visible && isMobile"
      ref="mobileTip"
      class="mt-4 rounded-2xl border border-slate-200 bg-white p-4 shadow-sm"
      @pointerdown.stop
      @click.stop
    >
      <div class="text-base font-semibold text-slate-900">
        {{ tooltip.region?.name }}
      </div>

      <div class="mt-1 text-sm text-blue-600 font-medium">
        {{ tooltip.region?.tenders }} тендеров
      </div>

      <button
        type="button"
        class="mt-3 inline-flex w-full items-center justify-center rounded-lg bg-blue-600 px-3 py-2 text-xs font-semibold text-white transition hover:bg-blue-700"
      >
        Смотреть
      </button>
    </div>
  </div>
</template>

<style scoped>
:deep(svg) {
  display: block;
  width: 100%;
  height: auto;
  pointer-events: auto;
  user-select: none;
}

:deep(.region) {
  pointer-events: all;
  fill: #e5e7eb;
  stroke: rgba(15, 23, 42, 0.25);
  stroke-width: 1;
  cursor: pointer;
  transition:
    fill 120ms ease,
    transform 120ms ease;
}

:deep(.region:hover) {
  fill: #93c5fd;
}

:deep(.region.is-active) {
  fill: #2563eb;
}
</style>
