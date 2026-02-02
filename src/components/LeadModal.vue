<script setup>
import { watch, onBeforeUnmount } from "vue";

const props = defineProps({ open: { type: Boolean, default: false } });
const emit = defineEmits(["close"]);
const close = () => emit("close");

function onKey(e) {
  if (e.key === "Escape") close();
}

watch(
  () => props.open,
  (v) => {
    if (v) document.addEventListener("keydown", onKey);
    else document.removeEventListener("keydown", onKey);
  }
);

onBeforeUnmount(() => document.removeEventListener("keydown", onKey));
</script>

<template>
  <div v-if="open" class="fixed inset-0 z-50">
    <div class="absolute inset-0 bg-black/60" @click="close"></div>

    <div class="relative mx-auto mt-16 w-[min(560px,calc(100%-32px))] rounded-2xl bg-white p-5 shadow-2xl">
      <button
        type="button"
        class="absolute right-3 top-3 grid h-9 w-9 place-items-center rounded-xl text-slate-500 hover:bg-slate-100"
        @click="close"
        aria-label="Закрыть"
      >
        ✕
      </button>

      <h2 class="text-xl font-extrabold text-slate-900">Заказать звонок / демо</h2>
      <p class="mt-2 text-sm text-slate-600">
        Оставьте контакты — мы свяжемся с вами и покажем сервис.
      </p>

      <form class="mt-4 grid gap-3" @submit.prevent="close">
        <label class="grid gap-1">
          <span class="text-xs font-semibold text-slate-600">Имя</span>
          <input class="h-11 rounded-xl border border-slate-200 px-3 outline-none focus:border-blue-400 focus:ring-4 focus:ring-blue-100" required />
        </label>

        <label class="grid gap-1">
          <span class="text-xs font-semibold text-slate-600">Телефон</span>
          <input class="h-11 rounded-xl border border-slate-200 px-3 outline-none focus:border-blue-400 focus:ring-4 focus:ring-blue-100" type="tel" required />
        </label>

        <label class="grid gap-1">
          <span class="text-xs font-semibold text-slate-600">Email</span>
          <input class="h-11 rounded-xl border border-slate-200 px-3 outline-none focus:border-blue-400 focus:ring-4 focus:ring-blue-100" type="email" />
        </label>

        <button class="btn-primary mt-1" type="submit">Отправить</button>

        <p class="text-[11px] text-slate-500">
          Нажимая «Отправить», вы соглашаетесь на обработку данных.
        </p>
      </form>
    </div>
  </div>
</template>
