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
        <a href="mailto:oscarfiscal25@gmail.com" target="_blank">Correo</a>
        <a href="https://github.com/oscarfiscal" target="_blank">GitHub</a>
        <!-- Puedes agregar más como LinkedIn -->
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
