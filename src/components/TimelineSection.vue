<template>
  <section
    ref="sectionRef"
    class="relative py-20 md:py-32 flex flex-col w-full m-auto overflow-hidden bg-base-100"
    aria-label="Timeline"
  >
    <!-- Background decoration -->
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <div class="absolute top-1/4 left-0 w-72 h-72 bg-blue-400/10 rounded-full blur-3xl"></div>
      <div class="absolute bottom-1/4 right-0 w-72 h-72 bg-indigo-400/10 rounded-full blur-3xl"></div>
    </div>

    <div ref="titleRef" class="relative z-10 text-center mb-16">
      <h2 class="text-4xl md:text-5xl lg:text-6xl font-nunito font-extrabold mb-4 bg-gradient-to-r from-slate-800 via-blue-700 to-indigo-700 dark:from-white dark:via-blue-200 dark:to-indigo-200 bg-clip-text text-transparent">
        In a nutshell
      </h2>
      <div class="w-24 h-1 bg-gradient-to-r from-blue-500 to-indigo-500 mx-auto rounded-full"></div>
    </div>

    <div class="relative mt-10">
      <!-- Left Arrow -->
      <button
        v-if="canScrollLeft"
        @click="scrollLeft"
        @keydown.enter="scrollLeft"
        @keydown.space.prevent="scrollLeft"
        class="absolute left-0 top-1/2 -translate-y-1/2 z-10 btn btn-circle btn-primary btn-sm lg:btn-md shadow-lg"
        aria-label="Scroll timeline left"
        type="button"
      >
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-5 h-5 lg:w-6 lg:h-6">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
        </svg>
      </button>

      <!-- Right Arrow -->
      <button
        v-if="canScrollRight"
        @click="scrollRight"
        @keydown.enter="scrollRight"
        @keydown.space.prevent="scrollRight"
        class="absolute right-0 top-1/2 -translate-y-1/2 z-10 btn btn-circle btn-primary btn-sm lg:btn-md shadow-lg"
        aria-label="Scroll timeline right"
        type="button"
      >
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-5 h-5 lg:w-6 lg:h-6">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
        </svg>
      </button>

      <!-- Timeline Container -->
      <div
        ref="timelineContainer"
        class="flex overflow-x-auto scroll-smooth snap-x snap-mandatory gap-4 px-12 md:px-16 pb-4 scrollbar-hide"
        @scroll="updateScrollButtons"
        @keydown.left.prevent="scrollLeft"
        @keydown.right.prevent="scrollRight"
        tabindex="0"
        role="region"
        aria-label="Timeline events"
      >
        <article
          v-for="(item, index) in timelineItems"
          :key="index"
          :ref="el => { if (el) timelineItemRefs[index] = el }"
          class="flex-shrink-0 w-80 md:w-96 snap-center"
        >
          <div class="card bg-base-200 shadow-xl hover:shadow-2xl transition-shadow duration-300 h-full">
            <div class="card-body">
              <div class="badge badge-primary badge-lg mb-2">
                {{ item.time }}
              </div>
              <h3 class="card-title text-xl md:text-2xl">
                {{ item.title }}
              </h3>
              <figure v-if="item.image" class="mt-2">
                <img
                  :src="item.image"
                  :alt="item.imageAlt || item.title"
                  class="rounded-xl w-full"
                  loading="lazy"
                >
              </figure>
              <div class="text-base leading-relaxed" v-html="item.description"></div>
            </div>
          </div>
        </article>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { useMotion } from '@vueuse/motion'

const sectionRef = ref(null)
const titleRef = ref(null)
const timelineContainer = ref(null)
const timelineItemRefs = ref([])
const canScrollLeft = ref(false)
const canScrollRight = ref(true)

const timelineItems = [
  {
    time: '1985',
    title: 'Born in Germany 🇩🇪',
    image: '/assets/timeline/born2.webp',
    imageAlt: 'Born in Germany',
    description: 'I was born in Germany and somewhat always fascinated by computers from a young age. Mostly I played with my dad\'s Apple - battled my sister in games like PacMan or Corvette.'
  },
  {
    time: '2009',
    title: 'Bachelor in Salzburg 🇦🇹',
    image: '/assets/timeline/fh.webp',
    imageAlt: 'Salzburg',
    description: 'I taught myself how to code and at the age of 15, I helped small companies to build websites.<br />Looking back, I am amazed how far I got as I didn\'t know much about "real programming". I believe I always had a "get it done" approach.<br />I moved to Salzburg, Austria to study and learn how to do it properly.'
  },
  {
    time: '2012+',
    title: 'Startup life',
    image: '/assets/timeline/startup1.webp',
    imageAlt: 'Startup life',
    description: 'So far I have mostly worked for early-stage startups and have been part of the journey of many companies. It fits me as I am a curious person and love to learn new things when wearing many hats.'
  },
  {
    time: '2015',
    title: 'AirConsole - Zurich 🇨🇭',
    image: '/assets/timeline/airconsole.webp',
    imageAlt: 'AirConsole',
    description: 'I joined AirConsole as founding member and helped to build the company from the ground up. I also showcased my game Tic Tac Boom at Gamescom that year, which has been played by over 10M players.'
  },
  {
    time: 'Hobbies',
    title: 'Traveling the world 🌍',
    image: '/assets/timeline/madeira.webp',
    imageAlt: 'Traveling the world',
    description: 'If you ask me what I spend my money on, it\'s mostly traveling. I love to explore new places, get surprised and go out of my comfort zone.'
  },
  {
    time: 'Life',
    title: 'Married',
    image: '/assets/timeline/married.webp',
    imageAlt: 'Married',
    description: 'While traveling the world, I met a Mexican girl - we are married since 2023. I spent quite some time in Mexico.'
  },
  {
    time: '2021',
    title: 'Nomad life',
    image: '/assets/timeline/nomad.webp',
    imageAlt: 'Nomad life',
    description: 'After the pandemic hit, I quit my flat, bought a Jeep and became a digital nomad. I loved the challenge and freedom of it. If I wanted to eat Pizza in Italy, it simply became my next destination.'
  },
  {
    time: '2023',
    title: 'Dibulo - Founder',
    image: '/assets/timeline/dibulo.webp',
    imageAlt: 'Dibulo',
    description: 'Wearing many hats, helped me to found my own company. In 2023 I went to a 6-week accelerator in Madeira to validate an idea: Dibulo - an interactive coloring session for kids. We launched shortly after and our coloring books became best-sellers on Amazon.nl.'
  },
  {
    time: '2025',
    title: 'Puppy Life 🐶',
    image: '/assets/timeline/maverick.webp',
    imageAlt: 'Puppy',
    description: 'It was never the question if we would get a dog, but when. In 2025, we got Maverick - our super cute Maltipoo.'
  }
]

const scrollLeft = () => {
  if (timelineContainer.value) {
    timelineContainer.value.scrollBy({ left: -400, behavior: 'smooth' })
  }
}

const scrollRight = () => {
  if (timelineContainer.value) {
    timelineContainer.value.scrollBy({ left: 400, behavior: 'smooth' })
  }
}

const updateScrollButtons = () => {
  if (timelineContainer.value) {
    const { scrollLeft, scrollWidth, clientWidth } = timelineContainer.value
    canScrollLeft.value = scrollLeft > 0
    canScrollRight.value = scrollLeft < scrollWidth - clientWidth - 10
  }
}

onMounted(() => {
  // Animate title
  if (titleRef.value) {
    useMotion(titleRef.value, {
      initial: { opacity: 0, y: 30 },
      enter: { 
        opacity: 1, 
        y: 0, 
        transition: { duration: 800, ease: [0.16, 1, 0.3, 1] }
      }
    })
  }

  // Animate timeline items with stagger
  timelineItemRefs.value.forEach((item, index) => {
    if (item) {
      useMotion(item, {
        initial: { opacity: 0, scale: 0.9, y: 30 },
        enter: {
          opacity: 1,
          scale: 1,
          y: 0,
          transition: {
            duration: 600,
            delay: index * 100,
            ease: [0.16, 1, 0.3, 1]
          }
        }
      })
    }
  })

  updateScrollButtons()
  if (timelineContainer.value) {
    // Add touch/swipe support
    let touchStartX = 0
    let touchEndX = 0

    timelineContainer.value.addEventListener('touchstart', (e) => {
      touchStartX = e.changedTouches[0].screenX
    })

    timelineContainer.value.addEventListener('touchend', (e) => {
      touchEndX = e.changedTouches[0].screenX
      handleSwipe()
    })

    const handleSwipe = () => {
      const diff = touchStartX - touchEndX
      if (Math.abs(diff) > 50) {
        if (diff > 0) {
          scrollRight()
        } else {
          scrollLeft()
        }
      }
    }

    timelineContainer.value.addEventListener('scroll', updateScrollButtons)
  }
})

onUnmounted(() => {
  if (timelineContainer.value) {
    timelineContainer.value.removeEventListener('scroll', updateScrollButtons)
  }
})
</script>

<style scoped>
.scrollbar-hide {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.scrollbar-hide::-webkit-scrollbar {
  display: none;
}
</style>

