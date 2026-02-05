<script setup>
const year = new Date().getFullYear()
</script>

<template>
  <footer class="">
    <div
      class="container-app mt-8 flex flex-col items-center gap-4 border-t border-slate-200 py-10 pt-6 text-center sm:flex-row sm:items-center sm:justify-between sm:text-left"
    >
      <img src="../assets/icons/Tenderbot.svg" />
      <div class="text-xs text-slate-500">© {{ year }} Tenderbot.kz. Все права защищены.</div>

      <ul class="space-y-2 text-sm text-slate-600">
        <li class="flex flex-col gap-1">
          <span class="font-bold text-slate-500">+7 747 094 42 15</span>
          <a
            class="inline-flex items-center gap-2 font-semibold text-blue-600 hover:text-blue-700 transition"
            href="tel:+77470944215"
          >
            <img src="../assets/icons/call.svg" alt="" class="h-4 w-4" aria-hidden="true" />
            Заказать звонок
          </a>
        </li>
      </ul>

      <div class="flex items-center gap-3 text-xs">
        <a
          class="inline-flex items-center gap-2 text-slate-700 hover:text-slate-900 transition"
          href="https://api.whatsapp.com/send/?phone=77470942065&text&type=phone_number&app_absent=0"
          target="_blank"
          rel="noopener noreferrer"
          aria-label="WhatsApp"
        >
          <img src="../assets/contacts/whatsApp.svg" alt="" class="h-7 w-7" aria-hidden="true" />
        </a>
        <a
          class="text-slate-500 hover:text-slate-900"
          href="https://t.me/tenderbot_kz"
          target="_blank"
          rel="noopener noreferrer"
          aria-label="Telegram"
        >
          <img src="../assets/contacts/telegram.svg" alt="" class="h-7 w-7" aria-hidden="true" />
        </a>
        <a
          class="text-slate-500 hover:text-slate-900"
          href="https://www.facebook.com/tenderbotkz"
          target="_blank"
          rel="noopener noreferrer"
          aria-label="facebook"
        >
          <img src="../assets/contacts/facebook.svg" alt="" class="h-7 w-7" aria-hidden="true" />
        </a>
        <a
          class="text-slate-500 hover:text-slate-900"
          href="https://instagram.com/tenderbotkz"
          target="_blank"
          rel="noopener noreferrer"
          aria-label="Instagram"
        >
          <img src="../assets/contacts/instagrem.svg" alt="" class="h-7 w-7" aria-hidden="true" />
        </a>

        <a
          class="text-slate-500 hover:text-slate-900"
          href="https://www.youtube.com/c/TENDERBOTKZ"
          target="_blank"
          rel="noopener noreferrer"
          aria-label="youtube"
        >
          <img src="../assets/contacts/youtube.svg" alt="" class="h-7 w-7" aria-hidden="true" />
        </a>
      </div>
    </div>
  </footer>
</template>
