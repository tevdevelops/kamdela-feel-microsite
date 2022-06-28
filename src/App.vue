<script setup>
import { ref, reactive, computed, onMounted } from 'vue'
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup

import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

const scroll = ref(0)

const is_midnight = computed(() => {
  return scroll.value >= 2000
})

const rotation = computed(() => {
  return (scroll.value * 360) / 2000
})

const timeText = computed(() => {
  return is_midnight.value ? '12:00 AM' : '11:59 PM'
})

const styleObject = computed(() => {
  return {
    transform: 'rotate(' + rotation.value + 'deg)',
  }
})

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

function handleScroll() {
  scroll.value = window.pageYOffset
}
</script>

<template>
  <div class="time">
    <h2 class="time__text">{{ timeText }}</h2>
  </div>

  <div class="clock">
    <svg
      class="clock__svg"
      :style="styleObject"
      viewBox="0 0 125 125"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
    >
      <circle
        class="clock__border"
        cx="62.5"
        cy="62.5"
        r="61"
        stroke="#F7D9D4"
        stroke-width="3"
      />
      <line
        class="clock__sec"
        x1="63"
        x2="63"
        y2="62"
        stroke="#F7D9D4"
        stroke-width="2"
      />
    </svg>
  </div>
  <div class="film-overlay" :style="{ opacity: scroll / 2000 }"></div>
</template>

<style>
@import url('./assets/styles/resets.css');
@import url('./assets/styles/fonts.css');

:root {
  --orange: #ff5f26;
  --blue: #394378;
  --lightPink: #f7d9d4;
  --background: -1;
}

body {
  background: black url(/src/assets/images/film-overlay.jpg);
  font-family: 'Resale Regular', serif;
  -webkit-font-smoothing: antialiased;
  text-align: center;
  color: var(--lightPink);
}

#app {
  min-height: calc(100vh + 2000px);
}

.film-overlay {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: var(--orange) url(/src/assets/images/film-overlay.jpg);
  background-blend-mode: screen;
  background-size: cover;
  z-index: var(--background);
}

.film-overlay__img {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  object-fit: cover;
}

.time {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.time__text {
  font-size: 90px;
  line-height: 108px;
}

.clock {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 50px;
}

.clock__svg {
  width: 100px;
  height: 100px;
}
</style>
