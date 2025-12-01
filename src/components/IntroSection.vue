<template>
  <section 
    ref="sectionRef"
    class="relative flex flex-col w-full min-h-screen justify-center items-center overflow-hidden bg-gradient-to-br from-slate-50 via-blue-50 to-indigo-100 dark:from-slate-900 dark:via-blue-900 dark:to-indigo-900"
  >
    <!-- Animated background elements -->
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <div 
        ref="bgCircle1"
        class="absolute w-96 h-96 bg-blue-400/20 rounded-full blur-3xl -top-48 -left-48"
      ></div>
      <div 
        ref="bgCircle2"
        class="absolute w-96 h-96 bg-indigo-400/20 rounded-full blur-3xl -bottom-48 -right-48"
      ></div>
    </div>

    <div class="relative z-10 flex flex-col md:flex-row justify-center items-center w-[90%] lg:w-[80%] py-20 lg:space-x-16">
      <!-- Image with animation -->
      <div 
        ref="imageRef"
        class="w-full lg:w-[40%] flex flex-col justify-center items-center mb-10 lg:mb-0"
      >
        <div class="relative group">
          <div class="absolute -inset-4"></div>
          <img
            class="relative max-w-[100%] md:max-w-[90%] lg:max-w-[100%] transform transition-transform duration-300 group-hover:scale-105"
            src="/assets/intro.webp"
            alt="François Weber"
            loading="eager"
          >
        </div>
      </div>

      <!-- Text content with staggered animations -->
      <div class="w-full lg:w-[60%] flex flex-col justify-center text-left space-y-6">
        <div ref="titleRef">
          <h1 class="text-3xl lg:text-5xl font-nunito font-extrabold bg-gradient-to-r from-blue-600 via-indigo-600 to-purple-600 bg-clip-text text-transparent leading-tight">
            Hi, I am François!
          </h1>
        </div>
        
        <div ref="textRef" class="space-y-4 text-base md:text-lg text-slate-700 dark:text-slate-300 leading-relaxed">
          <p class="opacity-90">
            I got my first computer with internet access at the age of 14, basically when the browser wars started and guestbooks were a thing.
            I was amazed about the possibilities — you could reach millions of people with just coding skills and an idea, right?!
          </p>
          <p class="opacity-90">
            Fast forward a few startups later and I learned it's not just the idea. Execution, timing and product–market fit matter just as much.
          </p>
          <p class="opacity-90">
            I'm a software engineer and founder at heart, with over a decade of experience building products and helping early-stage companies grow or founding my own.
            I love turning ideas into things people can use.
          </p>
          <p class="opacity-90">
            These days, I balance work with traveling, hanging out with my dog and 
            <a href="https://franzeus.medium.com/" target="_blank" rel="noopener noreferrer" class="text-blue-600 dark:text-blue-400 font-semibold hover:text-indigo-600 dark:hover:text-indigo-400 transition-colors underline decoration-2 underline-offset-4">occasionally I write</a> or
            <a href="https://www.youtube.com/channel/UCJvSxu2KCWQ4n7tzJFDgaQg" target="_blank" rel="noopener noreferrer" class="text-blue-600 dark:text-blue-400 font-semibold hover:text-indigo-600 dark:hover:text-indigo-400 transition-colors underline decoration-2 underline-offset-4">
              tell stories with gen AI</a>.
          </p>
        </div>
        
        <div ref="contactRef" class="flex flex-wrap items-center gap-4 pt-4">
          <a 
            href="mailto:hi@airfrancois.com" 
            class="btn btn-primary btn-lg"
          >
            Get in Touch
          </a>
          <a 
            href="https://www.linkedin.com/in/fcweber/" 
            target="_blank" 
            rel="noopener noreferrer" 
            class="btn btn-outline btn-lg"
          >
            LinkedIn
          </a>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { useMotion } from '@vueuse/motion'
import { ref, onMounted } from 'vue'

const sectionRef = ref(null)
const imageRef = ref(null)
const titleRef = ref(null)
const textRef = ref(null)
const contactRef = ref(null)
const bgCircle1 = ref(null)
const bgCircle2 = ref(null)

onMounted(() => {
  // Animate background circles
  if (bgCircle1.value) {
    useMotion(bgCircle1.value, {
      initial: { scale: 0, opacity: 0 },
      enter: { 
        scale: 1, 
        opacity: 1,
        transition: { duration: 2000, ease: 'easeOut' }
      }
    })
  }
  
  if (bgCircle2.value) {
    useMotion(bgCircle2.value, {
      initial: { scale: 0, opacity: 0 },
      enter: { 
        scale: 1, 
        opacity: 1,
        transition: { duration: 2000, delay: 200, ease: 'easeOut' }
      }
    })
  }

  // Animate image
  if (imageRef.value) {
    useMotion(imageRef.value, {
      initial: { opacity: 0, scale: 0.8, x: -50 },
      enter: { 
        opacity: 1, 
        scale: 1, 
        x: 0,
        transition: { duration: 800, ease: [0.16, 1, 0.3, 1] }
      }
    })
  }

  // Animate title
  if (titleRef.value) {
    useMotion(titleRef.value, {
      initial: { opacity: 0, y: 30 },
      enter: { 
        opacity: 1, 
        y: 0,
        transition: { duration: 600, delay: 200, ease: [0.16, 1, 0.3, 1] }
      }
    })
  }

  // Animate text
  if (textRef.value) {
    useMotion(textRef.value, {
      initial: { opacity: 0, y: 20 },
      enter: { 
        opacity: 1, 
        y: 0,
        transition: { duration: 600, delay: 400, ease: [0.16, 1, 0.3, 1] }
      }
    })
  }

  // Animate contact buttons
  if (contactRef.value) {
    useMotion(contactRef.value, {
      initial: { opacity: 0, y: 20 },
      enter: { 
        opacity: 1, 
        y: 0,
        transition: { duration: 600, delay: 600, ease: [0.16, 1, 0.3, 1] }
      }
    })
  }
})
</script>

