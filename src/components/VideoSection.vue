<template>
  <section
    ref="sectionRef"
    id="video-section"
    class="relative flex flex-col py-20 md:py-32 w-full m-auto bg-gradient-to-b from-black via-slate-900 to-black overflow-hidden"
    aria-label="Video content"
  >
    <!-- Animated background -->
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[800px] h-[800px] bg-blue-500/10 rounded-full blur-3xl"></div>
    </div>

    <div class="relative z-10 w-11/12 lg:w-2/4 m-auto">
      <div ref="titleRef" class="text-center mb-12">
        <h2 class="text-4xl md:text-5xl lg:text-6xl font-nunito font-extrabold mb-4 bg-gradient-to-r from-white via-blue-200 to-indigo-200 bg-clip-text text-transparent">
          I narrate stories with gen AI
        </h2>
        <div class="w-24 h-1 bg-gradient-to-r from-blue-500 to-indigo-500 mx-auto rounded-full"></div>
      </div>

      <div ref="videoRef" class="relative">
        <div
          v-if="shouldLoadVideo"
          class="aspect-video m-auto w-full rounded-2xl overflow-hidden shadow-2xl border-2 border-blue-500/30"
        >
          <iframe
            id="yt-video"
            class="w-full h-full"
            :src="videoSrc"
            title="Game Story"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
            referrerpolicy="strict-origin-when-cross-origin"
            allowfullscreen
            loading="lazy"
          />
        </div>
        <div
          v-else
          class="aspect-video m-auto w-full bg-gradient-to-br from-slate-800 to-slate-900 rounded-2xl flex items-center justify-center border-2 border-slate-700"
          aria-label="Video will load when scrolled into view"
        >
          <div class="text-center space-y-4">
            <div class="w-16 h-16 border-4 border-blue-500 border-t-transparent rounded-full animate-spin mx-auto"></div>
            <p class="text-slate-400 font-medium">Loading video...</p>
          </div>
        </div>
      </div>

      <div ref="buttonRef" class="mt-12 text-center">
        <a
          class="btn btn-primary btn-lg gap-2"
          href="https://www.youtube.com/channel/UCJvSxu2KCWQ4n7tzJFDgaQg"
          target="_blank"
          rel="noopener noreferrer"
          aria-label="Visit YouTube channel to see all videos"
        >
          See all videos
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-5 h-5">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
          </svg>
        </a>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { useMotion } from '@vueuse/motion'

const sectionRef = ref(null)
const titleRef = ref(null)
const videoRef = ref(null)
const buttonRef = ref(null)
const shouldLoadVideo = ref(false)
const videoSrc = 'https://www.youtube.com/embed/mQkUYhUog1g?si=sKvAeF9g7Nx_c_ar'

let observer = null

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

  // Animate video container
  if (videoRef.value) {
    useMotion(videoRef.value, {
      initial: { opacity: 0, scale: 0.9, y: 30 },
      enter: { 
        opacity: 1, 
        scale: 1,
        y: 0, 
        transition: { duration: 900, delay: 200, ease: [0.16, 1, 0.3, 1] }
      }
    })
  }

  // Animate button
  if (buttonRef.value) {
    useMotion(buttonRef.value, {
      initial: { opacity: 0, y: 20 },
      enter: { 
        opacity: 1, 
        y: 0, 
        transition: { duration: 700, delay: 400, ease: [0.16, 1, 0.3, 1] }
      }
    })
  }

  // Lazy load video when section comes into view
  if (sectionRef.value) {
    observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            shouldLoadVideo.value = true
            if (observer) {
              observer.disconnect()
            }
          }
        })
      },
      {
        root: null,
        rootMargin: '0px',
        threshold: 0.2
      }
    )

    observer.observe(sectionRef.value)
  }
})

onUnmounted(() => {
  if (observer) {
    observer.disconnect()
  }
})
</script>

