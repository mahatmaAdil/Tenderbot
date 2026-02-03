<script setup>
import { computed, nextTick, onMounted, onUnmounted, ref, watch } from 'vue'
import mapSvgRaw from '../assets/map.svg?raw'
import { regions } from '../data/regions'

const props = defineProps({
  modelValue: { type: String, default: '' },
})

const emit = defineEmits(['update:modelValue', 'select'])

const host = ref(null)
const activeId = computed(() => props.modelValue || '')

/* =====================
   TOOLTIP STATE
===================== */
const tooltip = ref({
  visible: false,
  x: 0,
  y: 0,
  region: null,
})

/* =====================
   DATA
===================== */
const regionMap = computed(() => {
  const m = new Map()
  regions.forEach((r) => m.set(r.svgId, r))
  return m
})

/* =====================
   ACTIVE REGION
===================== */
function setActive(id, name, tenders) {
  emit('update:modelValue', id)
  emit('select', { id, name, tenders })
}

/* =====================
   SVG CLASSES
===================== */
function applyActiveClass() {
  const root = host.value
  if (!root) return

  root.querySelectorAll('path[id]').forEach((p) => {
    p.classList.toggle('is-active', p.id === activeId.value)
  })
}

watch(activeId, applyActiveClass, { immediate: true })

/* =====================
   TOOLTIP LOGIC (CLICK)
===================== */
function openTooltip(e, region) {
  e.stopPropagation()

  tooltip.value.visible = true
  tooltip.value.x = e.clientX
  tooltip.value.y = e.clientY
  tooltip.value.region = region
}

function closeTooltip() {
  tooltip.value.visible = false
}

/* =====================
   MOUNT
===================== */
onMounted(async () => {
  await nextTick()

  const root = host.value
  if (!root) return

  const svg = root.querySelector('svg')
  if (svg) {
    svg.setAttribute('preserveAspectRatio', 'xMidYMid meet')

    if (!svg.getAttribute('viewBox')) {
      const w = parseFloat(svg.getAttribute('width') || '0')
      const h = parseFloat(svg.getAttribute('height') || '0')
      if (w > 0 && h > 0) svg.setAttribute('viewBox', `0 0 ${w} ${h}`)
    }
  }

  // ✅ ВОЗВРАЩАЕМ КЛАССЫ И КЛИКИ
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

    p.addEventListener('click', (e) => {
      e.stopPropagation()
      setActive(region.id, region.name, region.tenders)
      openTooltip(e, region)
    })
  })

  // ✅ клик вне карты закрывает tooltip
  document.addEventListener('click', closeTooltip)

  // ✅ подсветка выбранного региона
  applyActiveClass()
})

onUnmounted(() => {
  document.removeEventListener('click', closeTooltip)
})
</script>

<template>
  <div class="relative">
    <!-- CARD -->
    <div class="flex flex-col">
      <!-- HEADER -->
      <div class="flex items-center justify-between">
        <div>
          <div class="text-sm font-semibold text-slate-900">Карта тендеров в Казахстане</div>
          <div class="text-xs text-slate-500">
            Наведите курсор мыши на область, чтобы узнать о проводимых тендерах
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
      <div ref="host" class="mt-4 w-full" v-html="mapSvgRaw"></div>
    </div>

    <!-- TOOLTIP -->
    <div
      v-if="tooltip.visible"
      class="fixed z-50 min-w-[180px] rounded-xl bg-white px-3 py-3 text-xs shadow-xl ring-1 ring-black/10"
      :style="{ left: tooltip.x + 12 + 'px', top: tooltip.y + 12 + 'px' }"
      @click.stop
    >
      <div class="font-bold text-slate-900">
        {{ tooltip.region?.name }}
      </div>

      <div class="mt-1 text-lg font-bold text-blue-700">{{ tooltip.region?.tenders }} тендеров</div>

      <button
        type="button"
        class="mt-3 inline-flex w-full items-center justify-center rounded-lg bg-blue-600 px-3 py-2 text-xs font-semibold text-white transition hover:bg-blue-700"
        @click="onViewRegion"
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
