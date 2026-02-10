<script setup>
const emit = defineEmits(['open-lead'])
</script>

<template>
  <section class="final-cta relative overflow-hidden">
    <img
      src="@/assets/cta/card-cta.svg"
      alt=""
      class="pointer-events-none absolute left-1/2 top-0 w-[2500px] max-w-none -translate-x-1/2"
    />
    <div class="relative container-app cta-inner">
      <div class="cta-card mx-auto text-center text-white">
        <h2 class="cta-title font-extrabold">
          Прямо сейчас более <span class="text-white">1000 </span>
          <span class="block sm:inline">актуальных тендеров </span>
          <span class="block sm:inline">по вашей нише</span>
        </h2>

        <p class="cta-desc mx-auto text-white/85">
          Покажем крупные тендеры и объясним, как начать работать с сервисом.
        </p>

        <div class="cta-actions flex justify-center">
          <button
            type="button"
            class="rounded-full bg-white font-semibold text-slate-900 transition hover:bg-white/95"
            @click="emit('open-lead')"
          >
            <span class="hidden sm:inline">Хочу посмотреть</span>
            <span class="inline sm:hidden">Узнать больше</span>
          </button>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.cta-inner {
  display: flex;
  align-items: center;
  padding-top: 0;
  padding-bottom: 0;
}

.cta-card {
  width: 100%;
  max-width: 360px;
  padding: 28px 24px;
}

.cta-title {
  font-size: 18px;
  line-height: 1.25;
}

.cta-desc {
  margin-top: 10px;
  max-width: 280px;
  font-size: 12px;
  line-height: 1.25;
}

.cta-actions {
  margin-top: 16px;
}

.cta-actions button {
  width: 100%;
  max-width: 280px;
  padding: 12px 0;
  font-size: 14px;
}

@media (min-width: 640px) {
  .final-cta {
    min-height: auto;
  }

  .cta-inner {
    min-height: auto;
    display: block;
    padding-top: 56px;
    padding-bottom: 56px;
  }

  .cta-card {
    max-width: 900px;
    padding: 56px 40px;
  }

  .cta-title {
    font-size: 30px;
    line-height: 1.15;
  }

  .cta-desc {
    margin-top: 16px;
    max-width: 520px;
    font-size: 16px;
    line-height: 1.4;
  }

  .cta-actions {
    margin-top: 32px;
  }

  .cta-actions button {
    width: auto;
    max-width: none;
    padding: 12px 32px;
    font-size: 14px;
  }
}
</style>
