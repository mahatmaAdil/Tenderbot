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
const tendersLoading = ref(false)
const tendersError = ref('')

let svgEl = null

const norm = (s) =>
  String(s ?? '')
    .trim()
    .toLowerCase()

function updateIsMobile() {
  isMobile.value = window.innerWidth < 768
}

function closeTooltip() {
  tooltip.value.visible = false
  tooltip.value.region = null
}

function setActive(id, name, tenders, url) {
  emit('update:modelValue', id)
  emit('select', { id, name, tenders, url })
}

const regionMap = computed(() => {
  const m = new Map()
  for (const r of regions) m.set(r.id, r)
  return m
})

const katoToId = computed(() => {
  const m = new Map()
  for (const r of regions) {
    if (r.xml_id != null) m.set(String(r.xml_id), r.id)
  }
  return m
})

const regionIdByMapName = computed(() => {
  const m = new Map()
  for (const r of regions) {
    if (r.mapName) m.set(norm(r.mapName), r.id)
  }
  return m
})

const tendersById = ref(new Map())

function getMapNameById(id) {
  return regionMap.value.get(id)?.mapName || id
}

function getTendersByRegionId(id) {
  return Number(tendersById.value.get(String(id)) ?? regionMap.value.get(id)?.tenders ?? 0)
}

async function loadTenders() {
  tendersLoading.value = true
  tendersError.value = ''

  try {
    const res = await fetch('https://api.tenderbot.kz/api/promo/kato-list', {
      headers: { Accept: 'application/json' },
    })
    if (!res.ok) throw new Error(`HTTP ${res.status}`)

    const data = await res.json()
    const arr = Array.isArray(data) ? data : Array.isArray(data?.data) ? data.data : []

    const next = new Map()

    for (const item of arr) {
      const rawId = item?.xml_id
      if (rawId == null) continue

      const mappedId = katoToId.value.get(String(rawId))
      if (!mappedId) continue

      next.set(String(mappedId), Number(item?.lots_count ?? 0))
    }

    tendersById.value = next

    if (tooltip.value.visible && tooltip.value.region?.id) {
      const id = tooltip.value.region.id
      tooltip.value.region = {
        ...tooltip.value.region,
        tenders: getTendersByRegionId(id),
      }
    }
  } catch (e) {
    tendersError.value = e?.message || 'Не удалось загрузить тендеры'
  } finally {
    tendersLoading.value = false
  }
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

function handleRegionClick(id, e) {
  const r = regionMap.value.get(id)

  const region = {
    id,
    name: r?.name || id,
    tenders: getTendersByRegionId(id),
    url: r?.url || '',
  }

  setActive(region.id, region.name, region.tenders, region.url)

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

function addCityHitCircles(paths) {
  const cityNames = ['Астана', 'Алматы', 'Шымкент']

  const hitLayer = document.createElementNS('http://www.w3.org/2000/svg', 'g')
  hitLayer.setAttribute('data-hit-layer', 'true')
  svgEl.appendChild(hitLayer)

  for (const cityName of cityNames) {
    const id = regionIdByMapName.value.get(norm(cityName))
    if (!id) continue

    const path = Array.from(paths).find((p) => p.id === id)
    if (!path) continue

    const cfg = regionMap.value.get(id) || {}
    const bb = path.getBBox()

    const cx = bb.x + bb.width / 2 + Number(cfg.hitDx || 0)
    const cy = bb.y + bb.height / 2 + Number(cfg.hitDy || 0)
    const rr = Number(cfg.hitR || (isMobile.value ? 26 : 20))

    const circle = document.createElementNS('http://www.w3.org/2000/svg', 'circle')
    circle.setAttribute('cx', cx)
    circle.setAttribute('cy', cy)
    circle.setAttribute('r', rr)
    circle.setAttribute('fill', 'rgba(59,130,246,0.15)')
    circle.setAttribute('stroke', 'rgba(59,130,246,0.65)')
    circle.setAttribute('stroke-width', '2')
    circle.style.cursor = 'pointer'

    hitLayer.appendChild(circle)

    circle.addEventListener('click', (e) => {
      e.stopPropagation()
      handleRegionClick(id, e)
    })
  }
}

function addRegionLabels(paths) {
  for (const path of paths) {
    const id = path.id
    const label = getMapNameById(id)
    if (!label || label === id) continue

    const cfg = regionMap.value.get(id) || {}
    const bb = path.getBBox()

    const text = document.createElementNS('http://www.w3.org/2000/svg', 'text')
    text.setAttribute('x', bb.x + bb.width / 2 + Number(cfg.labelDx || 0))
    text.setAttribute('y', bb.y + bb.height / 2 + Number(cfg.labelDy || 0))
    text.setAttribute('text-anchor', 'middle')
    text.setAttribute('dominant-baseline', 'middle')
    text.setAttribute('pointer-events', 'none')
    text.setAttribute('fill', '#0f172a')
    text.setAttribute('font-size', '13')
    text.setAttribute('font-weight', '700')
    text.textContent = label

    svgEl.appendChild(text)
  }
}

onMounted(async () => {
  updateIsMobile()
  window.addEventListener('resize', updateIsMobile)
  document.addEventListener('pointerdown', onDocPointerDown)

  loadTenders()
  await nextTick()

  const root = host.value
  if (!root) return

  svgEl = root.querySelector('svg')
  if (!svgEl) return

  if (!svgEl.getAttribute('viewBox')) {
    svgEl.setAttribute('viewBox', '0 50 1000 500')
  }

  const paths = root.querySelectorAll('path[id]')

  paths.forEach((p) => {
    p.classList.add('region')
    p.addEventListener('click', (e) => {
      e.stopPropagation()
      handleRegionClick(p.id, e)
    })
  })

  addCityHitCircles(paths)
  addRegionLabels(paths)
  applyActiveClass()
})
function onWatchClick() {
  const url = tooltip.value.region?.url
  if (!url) return

  window.open(url, '_blank', 'noopener')
}

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
        <div class="font-regular text-slate-900">
          {{ tooltip.region?.name }}
        </div>

        <div class="mt-1 text-lg font-semibold text-[#078EE6]">
          {{ tooltip.region?.tenders }} тендеров
        </div>

        <button
          type="button"
          class="mt-3 inline-flex w-full items-center justify-center rounded-lg bg-[#078EE6] px-3 py-2 text-xs font-semibold text-white transition hover:bg-blue-700 disabled:bg-slate-200 disabled:text-slate-500 disabled:cursor-not-allowed"
          :disabled="!tooltip.region?.url"
          @click="onWatchClick"
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
        class="mt-3 inline-flex w-full items-center justify-center rounded-lg bg-blue-600 px-3 py-2 text-xs font-semibold text-white transition hover:bg-blue-700 disabled:bg-slate-200 disabled:text-slate-500 disabled:cursor-not-allowed"
        :disabled="!tooltip.region?.url"
        @click="onWatchClick"
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
