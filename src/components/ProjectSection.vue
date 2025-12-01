<template>
  <article
    ref="articleRef"
    :class="[
      'relative w-full px-4 md:px-8 lg:px-16 py-20 md:py-32',
      index % 2 === 0 
        ? 'bg-gradient-to-br from-slate-900 via-slate-800 to-slate-900' 
        : 'bg-gradient-to-br from-slate-800 via-slate-900 to-slate-800'
    ]"
  >
    <!-- Background decoration based on index -->
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <div 
        v-if="index % 2 === 0"
        class="absolute top-0 right-0 w-96 h-96 bg-purple-500/10 rounded-full blur-3xl"
      ></div>
      <div 
        v-if="index % 2 === 0"
        class="absolute bottom-0 left-0 w-96 h-96 bg-blue-500/10 rounded-full blur-3xl"
      ></div>
      <div 
        v-if="index % 2 === 1"
        class="absolute top-0 left-0 w-96 h-96 bg-indigo-500/10 rounded-full blur-3xl"
      ></div>
      <div 
        v-if="index % 2 === 1"
        class="absolute bottom-0 right-0 w-96 h-96 bg-cyan-500/10 rounded-full blur-3xl"
      ></div>
    </div>

    <div class="relative z-10 max-w-7xl mx-auto">
      <!-- Project Header with fancy styling - NOW ABOVE IMAGE -->
      <header ref="headerRef" class="mb-12">
        <div class="flex flex-col md:flex-row md:items-start md:justify-between gap-6 mb-6">
          <div class="space-y-3">
            <h2 
              :class="[
                'text-4xl md:text-5xl lg:text-6xl font-nunito font-extrabold bg-clip-text text-transparent',
                index % 2 === 0 
                  ? 'bg-gradient-to-r from-white via-blue-200 to-indigo-200' 
                  : 'bg-gradient-to-r from-white via-purple-200 to-pink-200'
              ]"
            >
              {{ project.name }}
            </h2>
            <div class="flex flex-wrap items-center gap-4 text-lg md:text-xl text-slate-300">
              <span 
                :class="[
                  'px-4 py-2 backdrop-blur-sm rounded-lg border',
                  index % 2 === 0
                    ? 'bg-slate-800/50 border-slate-700/50'
                    : 'bg-slate-700/50 border-slate-600/50'
                ]"
              >
                {{ project.role }}
              </span>
              <span 
                :class="[
                  'px-4 py-2 backdrop-blur-sm rounded-lg border',
                  index % 2 === 0
                    ? 'bg-slate-800/50 border-slate-700/50'
                    : 'bg-slate-700/50 border-slate-600/50'
                ]"
              >
                {{ project.year }}
              </span>
              <span 
                v-if="project.state" 
                class="px-4 py-2 bg-gradient-to-r from-green-500/20 to-emerald-500/20 backdrop-blur-sm rounded-lg border border-green-500/30 text-green-300"
              >
                {{ project.state }}
              </span>
            </div>
          </div>
          <a
            v-if="project.url"
            :href="project.url"
            target="_blank"
            rel="noopener noreferrer"
            ref="buttonRef"
            :class="[
              'group/btn px-8 py-4 text-white rounded-xl font-semibold shadow-xl hover:shadow-2xl hover:scale-105 transition-all duration-300 flex items-center gap-2 border',
              index % 2 === 0
                ? 'bg-gradient-to-r from-blue-600 to-indigo-600 border-blue-400/30'
                : 'bg-gradient-to-r from-purple-600 to-pink-600 border-purple-400/30'
            ]"
            :aria-label="`Visit ${project.name} website`"
          >
            <span>Visit Project</span>
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-5 h-5 transform group-hover/btn:translate-x-1 transition-transform">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
            </svg>
          </a>
        </div>
      </header>

      <!-- Hero Image with glassmorphism overlay - NOW BELOW TITLE -->
      <div v-if="heroImage" ref="imageRef" class="mb-12 relative group">
        <div class="absolute -inset-1 bg-gradient-to-r from-blue-600 via-purple-600 to-indigo-600 rounded-2xl blur-lg opacity-30 group-hover:opacity-50 transition-opacity duration-500"></div>
        <div class="relative overflow-hidden rounded-2xl shadow-2xl">
          <img
            :src="heroImage"
            :alt="`${project.name} - Hero screenshot showing the main interface or key feature`"
            class="w-full h-auto object-cover transform transition-transform duration-700 group-hover:scale-105"
            loading="lazy"
          >
          <div class="absolute inset-0 bg-gradient-to-t from-black/60 via-transparent to-transparent"></div>
        </div>
      </div>

      <!-- Description with fade-in -->
      <div ref="descriptionRef" class="mb-10 text-lg md:text-xl leading-relaxed text-slate-300 space-y-4">
        <p v-for="(paragraph, index) in descriptionParagraphs" :key="index" class="opacity-90">
          {{ paragraph }}
        </p>
      </div>

      <!-- Tech Stack with animated badges -->
      <div v-if="project.techStack && project.techStack.length > 0" ref="techRef" class="mb-12">
        <h3 class="text-2xl font-bold mb-6 text-white flex items-center gap-3">
          <span 
            :class="[
              'w-1 h-8 rounded-full',
              index % 2 === 0
                ? 'bg-gradient-to-b from-blue-500 to-indigo-500'
                : 'bg-gradient-to-b from-purple-500 to-pink-500'
            ]"
          ></span>
          Tech Stack
        </h3>
        <div class="flex flex-wrap gap-3">
          <span
            v-for="(tech, index) in project.techStack"
            :key="index"
            class="px-4 py-2 bg-slate-800/60 backdrop-blur-sm text-slate-200 rounded-lg border border-slate-700/50 hover:border-blue-500/50 hover:bg-slate-800/80 transition-all duration-300 hover:scale-105 font-medium shadow-lg"
          >
            {{ tech }}
          </span>
        </div>
      </div>

      <!-- Media Gallery with fancy grid -->
      <div v-if="hasMedia" ref="galleryRef" class="mt-16">
        <h3 class="text-2xl font-bold mb-8 text-white flex items-center gap-3">
          <span 
            :class="[
              'w-1 h-8 rounded-full',
              index % 2 === 0
                ? 'bg-gradient-to-b from-purple-500 to-pink-500'
                : 'bg-gradient-to-b from-cyan-500 to-blue-500'
            ]"
          ></span>
          Gallery
        </h3>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          <!-- Images -->
          <div
            v-for="(image, index) in project.images"
            :key="`img-${index}`"
            class="group relative overflow-hidden rounded-xl shadow-xl hover:shadow-2xl transition-all duration-300"
          >
            <div class="absolute inset-0 bg-gradient-to-t from-black/60 via-transparent to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300 z-10"></div>
            <img
              :src="image"
              :alt="`${project.name} - Screenshot ${index + 1} showing project features or interface`"
              class="w-full h-auto object-cover transform transition-transform duration-500 group-hover:scale-110"
              loading="lazy"
            >
          </div>
          <!-- Videos -->
          <div
            v-for="(video, index) in project.videos"
            :key="`vid-${index}`"
            class="relative overflow-hidden rounded-xl shadow-xl aspect-video bg-slate-800"
          >
            <video
              :src="video"
              controls
              class="w-full h-full object-cover"
              :aria-label="`${project.name} - Video ${index + 1}`"
            >
              Your browser does not support the video tag.
            </video>
          </div>
        </div>
      </div>
    </div>
  </article>
</template>

<script setup>
import { computed, ref, onMounted } from 'vue'
import { useMotion } from '@vueuse/motion'

const props = defineProps({
  project: {
    type: Object,
    required: true
  },
  index: {
    type: Number,
    default: 0
  }
})

const articleRef = ref(null)
const imageRef = ref(null)
const headerRef = ref(null)
const descriptionRef = ref(null)
const techRef = ref(null)
const galleryRef = ref(null)
const buttonRef = ref(null)

const heroImage = computed(() => {
  return props.project.images && props.project.images.length > 0
    ? props.project.images[0]
    : null
})

const descriptionParagraphs = computed(() => {
  if (!props.project.description) return []
  return props.project.description.split('. ').filter(p => p.trim().length > 0).map(p => p.trim() + '.')
})

const hasMedia = computed(() => {
  const hasImages = props.project.images && props.project.images.length > 1
  const hasVideos = props.project.videos && props.project.videos.length > 0
  return hasImages || hasVideos
})

// Use intersection observer to trigger animations when element comes into view
const setupAnimation = (element, options) => {
  if (!element) return
  
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          useMotion(element, {
            initial: options.initial,
            enter: options.enter
          })
          observer.disconnect()
        }
      })
    },
    { threshold: 0.1 }
  )
  
  observer.observe(element)
}

onMounted(() => {
  // Animate article container
  setupAnimation(articleRef.value, {
    initial: { opacity: 0, y: 60 },
    enter: {
      opacity: 1,
      y: 0,
      transition: {
        duration: 800,
        delay: props.index * 150,
        ease: [0.16, 1, 0.3, 1]
      }
    }
  })

  // Animate hero image
  if (imageRef.value) {
    setupAnimation(imageRef.value, {
      initial: { opacity: 0, scale: 0.9, y: 30 },
      enter: {
        opacity: 1,
        scale: 1,
        y: 0,
        transition: {
          duration: 900,
          delay: props.index * 150 + 200,
          ease: [0.16, 1, 0.3, 1]
        }
      }
    })
  }

  // Animate header
  if (headerRef.value) {
    setupAnimation(headerRef.value, {
      initial: { opacity: 0, y: 30 },
      enter: {
        opacity: 1,
        y: 0,
        transition: {
          duration: 700,
          delay: props.index * 150 + 300,
          ease: [0.16, 1, 0.3, 1]
        }
      }
    })
  }

  // Animate description
  if (descriptionRef.value) {
    setupAnimation(descriptionRef.value, {
      initial: { opacity: 0, y: 20 },
      enter: {
        opacity: 1,
        y: 0,
        transition: {
          duration: 700,
          delay: props.index * 150 + 400,
          ease: [0.16, 1, 0.3, 1]
        }
      }
    })
  }

  // Animate tech stack
  if (techRef.value) {
    setupAnimation(techRef.value, {
      initial: { opacity: 0, x: -20 },
      enter: {
        opacity: 1,
        x: 0,
        transition: {
          duration: 700,
          delay: props.index * 150 + 500,
          ease: [0.16, 1, 0.3, 1]
        }
      }
    })
  }

  // Animate gallery
  if (galleryRef.value) {
    setupAnimation(galleryRef.value, {
      initial: { opacity: 0, y: 30 },
      enter: {
        opacity: 1,
        y: 0,
        transition: {
          duration: 800,
          delay: props.index * 150 + 600,
          ease: [0.16, 1, 0.3, 1]
        }
      }
    })
  }

  // Animate button
  if (buttonRef.value) {
    setupAnimation(buttonRef.value, {
      initial: { opacity: 0, scale: 0.8 },
      enter: {
        opacity: 1,
        scale: 1,
        transition: {
          duration: 600,
          delay: props.index * 150 + 350,
          ease: [0.16, 1, 0.3, 1]
        }
      }
    })
  }
})
</script>

<style scoped>
article {
  scroll-margin-top: 4rem;
}

/* Add subtle border between sections */
article:not(:last-child)::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 90%;
  height: 1px;
  background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.1), transparent);
}
</style>
