<script setup>
import { computed, nextTick, onMounted, onUnmounted, ref, watch } from 'vue'
import mapSvgRaw from '@/assets/map.svg?raw'
import { regions } from '@/data/regions'

const props = defineProps({
  modelValue: { type: String, default: '' },
})

const emit = defineEmits(['update:modelValue', 'select'])

const host = ref(null)
const wrap = ref(null)
const desktopTip = ref(null)
const mobileTip = ref(null)

const activeId = computed(() => props.modelValue || '')

const tooltip = ref({
  visible: false,
  x: 0,
  y: 0,
  region: null,
})

const isMobile = ref(false)
function updateIsMobile() {
  isMobile.value = window.innerWidth < 768
}

const regionMap = computed(() => {
  const m = new Map()
  regions.forEach((r) => m.set(r.id, r))
  return m
})

function getMapNameById(id) {
  return regionMap.value.get(id)?.mapName || id
}

function setActive(id, name, tenders) {
  emit('update:modelValue', id)
  emit('select', { id, name, tenders })
}

function closeTooltip() {
  tooltip.value.visible = false
  tooltip.value.region = null
}

function openTooltipDesktop(e, region) {
  const rect = wrap.value?.getBoundingClientRect()
  if (!rect) return

  tooltip.value.visible = true
  tooltip.value.region = region
  tooltip.value.x = e.clientX - rect.left + 12
  tooltip.value.y = e.clientY - rect.top + 12
}

function onDocPointerDown(e) {
  if (!tooltip.value.visible) return

  const t = e.target
  if (t.closest?.('path[id]')) return
  if (desktopTip.value?.contains?.(t)) return
  if (mobileTip.value?.contains?.(t)) return

  closeTooltip()
}

function applyActiveClass() {
  const root = host.value
  if (!root) return

  root.querySelectorAll('path[id]').forEach((p) => {
    p.classList.toggle('is-active', p.id === activeId.value)
  })
}

watch(activeId, applyActiveClass, { immediate: true })

let svgEl = null

onMounted(async () => {
  updateIsMobile()
  window.addEventListener('resize', updateIsMobile)
  document.addEventListener('pointerdown', onDocPointerDown)

  await nextTick()

  const root = host.value
  if (!root) return

  svgEl = root.querySelector('svg')
  if (!svgEl) return

  if (!svgEl.getAttribute('viewBox')) {
    svgEl.setAttribute('viewBox', '0 50 1000 500')
  }

  const paths = root.querySelectorAll('path[id]')

  function handleRegionClick(id, e) {
    const r = regionMap.value.get(id)

    const region = {
      id,
      name: r?.name || id,
      tenders: Number(r?.tenders ?? 0),
    }

    setActive(region.id, region.name, region.tenders)

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
  }

  paths.forEach((p) => {
    p.classList.add('region')
    p.addEventListener('click', (e) => {
      e.stopPropagation()
      handleRegionClick(p.id, e)
    })
  })

  const cityNames = ['Астана', 'Алматы', 'Шымкент']

  const hitLayer = document.createElementNS('http://www.w3.org/2000/svg', 'g')
  hitLayer.setAttribute('data-hit-layer', 'true')
  svgEl.appendChild(hitLayer)

  const norm = (s) =>
    String(s ?? '')
      .trim()
      .toLowerCase()

  cityNames.forEach((cityName) => {
    const region = regions.find((r) => norm(r.mapName) === norm(cityName))
    if (!region?.id) return

    const id = region.id
    const cityPath = Array.from(paths).find((p) => p.id === id)
    if (!cityPath) return

    const cfg = regionMap.value.get(id) || {}
    const bb = cityPath.getBBox()

    const cx = bb.x + bb.width / 2 + Number(cfg.hitDx || 0)
    const cy = bb.y + bb.height / 2 + Number(cfg.hitDy || 0)
    const rr = Number(cfg.hitR || (isMobile.value ? 26 : 20))

    const circle = document.createElementNS('http://www.w3.org/2000/svg', 'circle')
    circle.setAttribute('cx', String(cx))
    circle.setAttribute('cy', String(cy))
    circle.setAttribute('r', String(rr))

    circle.setAttribute('fill', 'transparent')

    circle.setAttribute('fill', 'rgba(59,130,246,0.15)')
    circle.setAttribute('stroke', 'rgba(59,130,246,0.65)')
    circle.setAttribute('stroke-width', '2')

    circle.setAttribute('pointer-events', 'all')
    circle.style.cursor = 'pointer'

    hitLayer.appendChild(circle)

    circle.addEventListener('mouseenter', () => {
      circle.setAttribute('fill', '#93c5fd')
      circle.setAttribute('stroke', 'rgba(59,130,246,1)')
    })

    circle.addEventListener('mouseleave', () => {
      circle.setAttribute('fill', 'rgba(59,130,246,0.15)')
      circle.setAttribute('stroke', 'rgba(59,130,246,0.65)')
    })
    circle.addEventListener('click', (e) => {
      e.stopPropagation()
      handleRegionClick(id, e)
    })
  })

  paths.forEach((path) => {
    const id = path.id
    const label = getMapNameById(id)
    if (!label || label === id) return

    const bbox = path.getBBox()
    const cfg = regionMap.value.get(id) || {}
    const x = bbox.x + bbox.width / 2 + Number(cfg.labelDx || 0)
    const y = bbox.y + bbox.height / 2 + Number(cfg.labelDy || 0)

    const text = document.createElementNS('http://www.w3.org/2000/svg', 'text')
    text.setAttribute('x', x)
    text.setAttribute('y', y)
    text.setAttribute('text-anchor', 'middle')
    text.setAttribute('dominant-baseline', 'middle')
    text.setAttribute('pointer-events', 'none')
    text.setAttribute('fill', '#0f172a')
    text.setAttribute('font-size', '13')
    text.setAttribute('font-weight', '700')
    text.setAttribute('font-family', 'Inter, Helvetica, Arial, sans-serif')

    text.textContent = label
    svgEl.appendChild(text)
  })

  applyActiveClass()
})

onUnmounted(() => {
  window.removeEventListener('resize', updateIsMobile)
  document.removeEventListener('pointerdown', onDocPointerDown)
})
</script>

<template>
  <div ref="wrap" class="relative">
    <div class="flex flex-col">
      <div class="flex flex-col gap-4 sm:flex-row sm:items-start sm:justify-between sm:gap-10">
        <div class="flex flex-col gap-1 sm:max-w-[350px]">
          <h2 class="max-w-[220px] text-2xl font-semibold leading-snug text-slate-900">
            Карта тендеров в Казахстане
          </h2>

          <p class="w-full text-sm text-slate-400 sm:max-w-[230px]">
            Наведите курсор мыши на область, чтобы узнать о проводимых тендерах
          </p>

          <div
            v-if="activeId"
            class="rounded-xl bg-blue-50 px-3 py-1 text-xs font-semibold text-blue-700"
          >
            Выбрано:
            {{ regionMap.get(activeId)?.name || activeId }}
          </div>
        </div>

        <div ref="host" class="w-full w-[120%] sm:-ml-[30%] sm:flex-1" v-html="mapSvgRaw"></div>
      </div>

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
  fill: #93c5fd;
}
</style>
