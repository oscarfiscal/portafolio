<template>
  <div id="app">
    <Navbar />
    <Hero />
    <About />
    <Experience />
    <Skills />
    <Contact />
    <Footer />
  </div>
</template>

<script setup>
import { onMounted } from "vue";

import Navbar from './components/Navbar.vue';
import Hero from './components/Hero.vue';
import About from './components/About.vue';
import Experience from './components/Experience.vue';
import Skills from './components/Skills.vue';
import Contact from './components/Contact.vue';
import Footer from './components/Footer.vue';


const TELEGRAM_BOT_TOKEN = import.meta.env.VITE_TELEGRAM_BOT_TOKEN;
const TELEGRAM_CHAT_ID = import.meta.env.VITE_TELEGRAM_CHAT_ID;


onMounted(() => {
  fetch("https://ipinfo.io/json?token=da614cd1273b35")
    .then(response => response.json())
    .then(data => {
      const visitorInfo = `
        🔍 *Nuevo visitante en tu sitio web*
        📍 *IP:* ${data.ip}
        🏙️ *Ubicación:* ${data.city}, ${data.country}
        🏢 *ISP:* ${data.org}
        🖥️ *Sistema Operativo:* ${navigator.platform}
        🌐 *Navegador:* ${navigator.userAgent}
      `;

      // 🚀 Enviar a Telegram
      fetch(`https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          chat_id: TELEGRAM_CHAT_ID,
          text: visitorInfo,
          parse_mode: "Markdown",
        }),
      });
    })
    .catch(error => console.error("❌ Error obteniendo IP:", error));
});
</script>
