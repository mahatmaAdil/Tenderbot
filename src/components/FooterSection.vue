<script setup>
const year = new Date().getFullYear()
</script>

<template>
  <footer class="border-t border-slate-200 bg-white">
    <div class="container-app py-10 sm:py-12">
      <div class="grid gap-8 sm:grid-cols-2 lg:grid-cols-4">
        <!-- Бренд/описание -->
        <div class="space-y-3">
          <div class="text-base font-extrabold text-slate-900">Tenderbot.kz</div>
          <p class="text-sm text-slate-600">
            Тендеры и госзакупки Казахстана в одном окне. Подбор, сопровождение и аналитика.
          </p>
        </div>

        <!-- Контакты -->
        <div class="space-y-3">
          <div class="text-sm font-bold text-slate-900">Контакты</div>
          <ul class="space-y-2 text-sm text-slate-600">
            <li>
              <span class="text-slate-500">Тел:</span>
              <a class="font-semibold text-slate-900 hover:underline" href="tel:+77000000000">
                +7 (700) 000-00-00
              </a>
            </li>
            <li>
              <span class="text-slate-500">Email:</span>
              <a
                class="font-semibold text-slate-900 hover:underline"
                href="mailto:info@tenderbot.kz"
              >
                info@tenderbot.kz
              </a>
            </li>
            <li class="text-slate-500">Казахстан</li>
          </ul>
        </div>

        <!-- Навигация -->
        <div class="space-y-3">
          <div class="text-sm font-bold text-slate-900">Разделы</div>
          <ul class="space-y-2 text-sm">
            <li><a class="text-slate-600 hover:text-slate-900" href="#platforms">Площадки</a></li>
            <li>
              <a class="text-slate-600 hover:text-slate-900" href="#features">Преимущества</a>
            </li>
            <li><a class="text-slate-600 hover:text-slate-900" href="#how">Как работает</a></li>
            <li><a class="text-slate-600 hover:text-slate-900" href="#cta">Демо</a></li>
          </ul>
        </div>

        <!-- Документы -->
        <div class="space-y-3">
          <div class="text-sm font-bold text-slate-900">Документы</div>
          <ul class="space-y-2 text-sm">
            <li>
              <a
                class="text-slate-600 hover:text-slate-900"
                href="#"
                aria-label="Политика конфиденциальности"
              >
                Политика конфиденциальности
              </a>
            </li>
            <li>
              <a
                class="text-slate-600 hover:text-slate-900"
                href="#"
                aria-label="Пользовательское соглашение"
              >
                Пользовательское соглашение
              </a>
            </li>
            <li>
              <a
                class="text-slate-600 hover:text-slate-900"
                href="#"
                aria-label="Согласие на обработку данных"
              >
                Согласие на обработку данных
              </a>
            </li>
          </ul>
        </div>
      </div>

      <!-- нижняя полоса -->
      <div
        class="mt-8 flex flex-col gap-3 border-t border-slate-200 pt-6 sm:flex-row sm:items-center sm:justify-between"
      >
        <div class="text-xs text-slate-500">© {{ year }} Tenderbot.kz. Все права защищены.</div>

        <div class="flex items-center gap-3 text-xs">
          <a class="text-slate-500 hover:text-slate-900" href="#" aria-label="Instagram"
            >Instagram</a
          >
          <span class="text-slate-300">•</span>
          <a class="text-slate-500 hover:text-slate-900" href="#" aria-label="Telegram">Telegram</a>
          <span class="text-slate-300">•</span>
          <a class="text-slate-500 hover:text-slate-900" href="#" aria-label="WhatsApp">WhatsApp</a>
        </div>
      </div>
    </div>
  </footer>
</template>
