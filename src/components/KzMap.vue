<script setup>
import { computed, nextTick, onMounted, ref, watch } from 'vue'
import mapSvgRaw from '../assets/map.svg?raw'
import { regions } from '../data/regions'

const props = defineProps({
  modelValue: { type: String, default: '' }, // выбранный регион снаружи (optional)
})

const emit = defineEmits(['update:modelValue', 'select'])

const host = ref(null)
const activeId = computed(() => props.modelValue || '')

watch(
  () => props.modelValue,
  () => applyActiveClass(),
  { immediate: true },
)

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

function setActive(id, name) {
  emit('update:modelValue', id)
  emit('select', { id, name }) // <-- важно: отдаём объект
  // подсветка обновится через watch
}

function applyActiveClass() {
  const root = host.value
  if (!root) return

  const paths = root.querySelectorAll('path[id]')
  paths.forEach((p) => {
    p.classList.toggle('is-active', p.id === (props.modelValue || ''))
  })
}

function showTooltip(e, id, name) {
  tooltip.value.visible = true
  tooltip.value.x = e.clientX
  tooltip.value.y = e.clientY
  tooltip.value.region = { id, name }
}

function moveTooltip(e) {
  if (!tooltip.value.visible) return
  tooltip.value.x = e.clientX
  tooltip.value.y = e.clientY
}

function hideTooltip() {
  tooltip.value.visible = false
}

onMounted(async () => {
  // ждём пока v-html вставит SVG
  await nextTick()

  const root = host.value
  if (!root) return

  // найдём все кликабельные path по id
  const paths = root.querySelectorAll('path[id]')

  paths.forEach((p) => {
    p.classList.add('region')

    const id = p.id
    const name = p.getAttribute('title') || id

    p.addEventListener('mouseenter', (e) => showTooltip(e, id, name))
    p.addEventListener('mousemove', (e) => moveTooltip(e))
    p.addEventListener('mouseleave', hideTooltip)
    p.addEventListener('click', () => setActive(id, name))
  })

  applyActiveClass()
})
</script>

<template>
  <div class="relative">
    <!-- карта -->
    <div class="card overflow-hidden p-4 sm:p-5">
      <div class="flex items-center justify-between">
        <div>
          <div class="text-sm font-semibold text-slate-900">Карта Казахстана</div>
          <div class="text-xs text-slate-500">Нажмите на регион</div>
        </div>

        <div
          v-if="activeId"
          class="rounded-xl bg-blue-50 px-3 py-1 text-xs font-semibold text-blue-700"
        >
          Выбрано: {{ regionMap.get(activeId)?.name || activeId }}
        </div>
      </div>

      <div class="mt-4 rounded-2xl bg-slate-50 p-3">
        <div ref="host" class="[&_svg]:w-auto [&_svg]:h-auto [&_svg]:block" v-html="mapSvgRaw" />
      </div>

      <div class="mt-4 text-xs text-slate-500">
        Подсказка: если какой-то регион не реагирует — проверь, что у его
        <code>path</code> есть <code>id</code>.
      </div>
    </div>

    <!-- tooltip -->
    <div
      v-if="tooltip.visible"
      class="pointer-events-none fixed z-50 min-w-[160px] rounded-xl bg-white px-3 py-2 text-xs shadow-xl ring-1 ring-black/10"
      :style="{ left: tooltip.x + 12 + 'px', top: tooltip.y + 12 + 'px' }"
    >
      <div class="font-bold text-slate-900">{{ tooltip.region?.name }}</div>
      <div class="text-slate-500">{{ tooltip.region?.tenders }} тендеров</div>
    </div>
  </div>
</template>

<style scoped>
/* базовая стилизация регионов */
:deep(svg) {
  user-select: none;
  pointer-events: none;
  width: 100%;
  height: 320px;
}
:deep(svg *) {
  pointer-events: auto;
}

:deep(.region) {
  fill: #e5e7eb;
  stroke: rgba(15, 23, 42, 0.25);
  stroke-width: 1;
  cursor: pointer;
  transition:
    fill 120ms ease,
    transform 120ms ease;
}

:deep(.region:hover) {
  fill: #93c5fd; /* голубой */
}

:deep(.region.is-active) {
  fill: #2563eb; /* синий */
}

:deep(.region) {
  pointer-events: all;
}
</style>
