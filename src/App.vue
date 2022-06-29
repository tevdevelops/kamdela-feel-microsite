<script setup>
import { ref, reactive, computed, onMounted, onUnmounted } from 'vue'
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

  // gsap.fromTo(
  //   '.sparkle',
  //   { scale: 0 },
  //   {
  //     scale: 3,
  //     repeat: -1,
  //     yoyo: true,
  //     duration: 0.8,
  //     repeatDelay: 9,
  // stagger: {
  //   from: 'random',
  //   amount: 10,
  //   each: 7,
  // },
  //   }
  // )

  //   gsap.to(".class", {
  //   x:"random([0, 100, 200, 500])" // randomly selects one of the values (0, 100, 200, or 500)
  // });

  let animationTimeline = gsap.timeline({
    scrollTrigger: {
      markers: false,
      trigger: '.container--scene-2',
      pin: true,
      toggleActions: 'play none resume reverse',
    },
  })

  // add animations and labels to the timeline
  animationTimeline
    .addLabel('hide-intro')
    .to('.container--scene-1', { opacity: 0 })
    .addLabel('show-feel-it')
    .to('.feel-it > .text-split', { opacity: 1, stagger: 0.5 })
    .addLabel('transform-feel-it')
    .to(
      '.feel-it__i, .feel-it__it',
      { opacity: 0, width: 0 },
      'transform-feel-it'
    )
    .to('.to-blue', { color: '#252457' }, 'transform-feel-it')
    .addLabel('translate-letters-1')
    .to('.feel-it__f', { yPercent: -100 }, 'translate-letters-1')
    .to('.feel-it__l', { yPercent: 100 }, 'translate-letters-1')
    .addLabel('translate-letters-2')
    .to('.feel-it__f', { xPercent: 100, duration: 0.3 }, 'translate-letters-2')
    .to('.feel-it__l', { xPercent: -108, duration: 0.3 }, 'translate-letters-2')
    .addLabel('translate-letters-3')
    .to('.feel-it__f', { yPercent: -50, duration: 0.3 }, 'translate-letters-3')
    .to('.feel-it__e1', { yPercent: 50, duration: 0.3 }, 'translate-letters-3')
    .to('.feel-it__e2', { yPercent: -50, duration: 0.4 }, 'translate-letters-3')
    .to('.feel-it__l', { yPercent: 50, duration: 0.4 }, 'translate-letters-3')
  // .addLabel('spin-feel-it')
  // .to('.feel-it__feel', { rotate: 360,  })
  // .addLabel('color')
  // .from('.box', { backgroundColor: '#28a92b' })
  // .addLabel('spin')
  // .to('.box', { rotation: 360 })
  // .addLabel('end')
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})

function handleScroll() {
  scroll.value = window.pageYOffset
}
</script>

<template>
  <div
    class="container"
    :class="{
      'is-scroll-locked': is_midnight,
    }"
  >
    <div class="sparkles">
      <img
        v-for="index in 9"
        :key="index"
        src="./assets/images/sparkle.png"
        class="sparkle"
      />
    </div>

    <div class="container--scene-1">
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
    </div>

    <div class="container--scene-2 flex-center">
      <h2 class="feel-it">
        <div class="text-split feel-it__i">I</div>
        <div class="text-split feel-it__feel">
          <div class="text-split to-blue feel-it__f">F</div>
          <div class="text-split feel-it__e1">E</div>
          <div class="text-split feel-it__e2">E</div>
          <div class="text-split to-blue feel-it__l">L</div>
        </div>
        <div class="text-split feel-it__it">It</div>
      </h2>
    </div>

    <div class="film-overlay" :style="{ opacity: scroll / 2000 }"></div>
  </div>
</template>

<style>
@import url('./assets/styles/resets.css');
@import url('./assets/styles/fonts.css');
@import url('./assets/styles/utilities.css');

:root {
  --orange: #ff5f26;
  --blue: #252457;
  --lightPink: #f7d9d4;
  --darkGrey: #333333;
  --background: -1;
  --midground: 5;
  --foreground: 10;
}

body {
  background: black url(/src/assets/images/film-overlay.jpg);
  font-family: 'Resale Regular', serif;
  -webkit-font-smoothing: antialiased;
  text-align: center;
  text-transform: uppercase;
  color: var(--lightPink);
}

.sparkles {
  visibility: hidden;
  position: fixed;
  z-index: var(--midground);
  width: 100%;
  height: 100%;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  align-items: center;
  justify-items: center;
}

.sparkle {
  width: 25px;
  height: auto;
}

.container--scene-1 {
  min-height: calc(100vh + 2000px);
}

.container--scene-2 {
  min-height: 100vh;
}

.container.is-scroll-locked {
  /* min-height: 100vh; */
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

/* .film-overlay__img {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  object-fit: cover;
} */

.time {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100%;
}

.time__text {
  font-size: 50px;
  line-height: 60px;
}

.clock {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 50px;
}

.clock__svg {
  width: 75px;
  height: 75px;
}

.feel-it {
  font-size: 45px;
}

.feel-it > .text-split {
  opacity: 0;
  margin: 0 10px;
}

@media (min-width: 700px) {
  /* .time__text {
    font-size: 90px;
    line-height: 108px;
  } */
}
@media (min-width: 1280px) {
  .time__text {
    font-size: 90px;
    line-height: 108px;
  }

  .feel-it {
    font-size: 90px;
    line-height: 90px;
  }

  .feel-it > .text-split {
    margin: 0 20px;
  }

  .clock__svg {
    width: 100px;
    height: 100px;
  }
}
</style>
