<template>
  <section id="contact">
    <h2>Contacto</h2>
    <div class="contact-section">
      <p>
        ¿Tienes un proyecto o necesitas reforzar tu equipo de desarrollo?
        ¡Envíame un mensaje!
      </p>
      <form @submit.prevent="sendTelegram">
        <label>
          Nombre:
          <input type="text" v-model="formName" required />
        </label>
        <label>
          Email:
          <input type="email" v-model="formEmail" required />
        </label>
        <label>
          Teléfono:
          <input
            type="tel"
            v-model="formPhone"
            placeholder="+57 3001234567"
            required
          />
        </label>
        <label>
          Mensaje:
          <textarea rows="4" v-model="formMessage" required></textarea>
        </label>
        <button type="submit">Enviar</button>
      </form>
      <p class="status" v-if="statusMsg">{{ statusMsg }}</p>

      <!-- Links de contacto directo -->
      <div class="contact-links">
        <p>O contáctame directamente en:</p>
        <div class="link-list">
          <a href="mailto:oscarfiscal25@gmail.com" target="_blank">
            <svg class="icon" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M12 13.065 1.2 6.6A2 2 0 0 1 3 4h18a2 2 0 0 1 1.8 2.6l-10.8 6.465ZM12 15l11-6v9a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2v-9l11 6Z"
              />
            </svg>
            <span>Correo</span>
          </a>

          <a href="https://github.com/oscarfiscal" target="_blank">
            <svg class="icon" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M12 2a10 10 0 0 0-3.162 19.493c.5.093.683-.217.683-.483 0-.237-.009-.868-.013-1.703-2.782.605-3.37-1.34-3.37-1.34-.454-1.15-1.11-1.457-1.11-1.457-.907-.62.069-.607.069-.607 1.002.07 1.53 1.03 1.53 1.03.892 1.528 2.341 1.087 2.91.831.092-.647.35-1.087.636-1.337-2.22-.252-4.555-1.11-4.555-4.943 0-1.092.39-1.987 1.029-2.687-.103-.253-.446-1.27.098-2.647 0 0 .84-.269 2.75 1.026A9.564 9.564 0 0 1 12 6.844c.85.004 1.705.115 2.504.337 1.909-1.295 2.748-1.026 2.748-1.026.546 1.378.202 2.395.1 2.648.64.7 1.028 1.595 1.028 2.687 0 3.842-2.338 4.687-4.566 4.934.36.31.68.924.68 1.861 0 1.343-.012 2.424-.012 2.752 0 .269.18.58.688.482A10.002 10.002 0 0 0 12 2Z"
              />
            </svg>
            <span>GitHub</span>
          </a>

          <a
            href="https://www.linkedin.com/in/oscar-fiscal-277a741a6"
            target="_blank"
          >
            <svg class="icon" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M20.45 20.45h-3.6v-5.4c0-1.28-.02-2.94-1.8-2.94s-2.07 1.4-2.07 2.84v5.5H9.4V9.9h3.46v1.44h.05c.48-.9 1.66-1.84 3.42-1.84 3.66 0 4.34 2.4 4.34 5.52v5.43zM5.34 8.44a2.09 2.09 0 1 1 0-4.18 2.09 2.09 0 0 1 0 4.18zM7.13 20.45H3.55V9.9h3.58v10.55zM22.24 0H1.76C.79 0 0 .77 0 1.72v20.55C0 23.24.8 24 1.76 24h20.48c.96 0 1.76-.77 1.76-1.72V1.72C24 .76 23.2 0 22.24 0z"
              />
            </svg>
            <span>LinkedIn</span>
          </a>
        </div>
      </div>

      <!-- Botón flotante de WhatsApp -->
      <a
        href="https://wa.me/573103195434?text=Hola%20Oscar,%20vi%20tu%20portafolio%20y%20quiero%20contactarte"
        class="whatsapp-float"
        target="_blank"
        rel="noopener"
      >
        <img src="/whatsapp.svg" alt="WhatsApp" />
      </a>
    </div>
  </section>
</template>

<script setup>
import { ref } from "vue";

const formName = ref("");
const formEmail = ref("");
const formPhone = ref("");
const formMessage = ref("");
const statusMsg = ref("");

// Ajusta estos valores a tu token y chat ID
const TELEGRAM_BOT_TOKEN = "8130090156:AAEadD9CoQofCsGhpjaWYI0YEJWOb5PDxXw";
const TELEGRAM_CHAT_ID = 6246067242;

const sendTelegram = async () => {
  statusMsg.value = "Enviando...";

  try {
    const textMessage =
      `*Nuevo mensaje de contacto*\n` +
      `Nombre: ${formName.value}\n` +
      `Email: ${formEmail.value}\n` +
      `Tel: ${formPhone.value}\n\n` +
      `${formMessage.value}`;

    const response = await fetch(
      `https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage`,
      {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          chat_id: TELEGRAM_CHAT_ID,
          text: textMessage,
          parse_mode: "Markdown",
        }),
      }
    );
    const data = await response.json();

    if (data.ok) {
      statusMsg.value = "Mensaje enviado correctamente.";
      formName.value = "";
      formEmail.value = "";
      formPhone.value = "";
      formMessage.value = "";
    } else {
      statusMsg.value = "Error al enviar: " + JSON.stringify(data);
    }
  } catch (err) {
    statusMsg.value = "Error de conexión: " + err.message;
  }
};
</script>
