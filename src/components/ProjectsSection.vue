<template>
  <section
    ref="sectionRef"
    class="relative py-32 flex flex-col w-full m-auto bg-gradient-to-b from-slate-900 via-slate-800 to-slate-900 text-white overflow-hidden"
    aria-label="Projects"
  >
    <!-- Animated background -->
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <div class="absolute top-0 left-1/4 w-96 h-96 bg-purple-500/10 rounded-full blur-3xl"></div>
      <div class="absolute bottom-0 right-1/4 w-96 h-96 bg-blue-500/10 rounded-full blur-3xl"></div>
    </div>

    <div class="relative z-10">
      <div ref="titleRef" class="text-center mb-20">
        <h2 class="text-4xl md:text-5xl lg:text-6xl font-nunito font-extrabold mb-4 bg-gradient-to-r from-white via-blue-200 to-indigo-200 bg-clip-text text-transparent">
          Some things I have worked on
        </h2>
        <div class="w-24 h-1 bg-gradient-to-r from-blue-500 to-indigo-500 mx-auto rounded-full"></div>
      </div>

      <div class="w-full">
        <ProjectSection
          v-for="(project, index) in visibleProjects"
          :key="project.name"
          :project="project"
          :index="index"
        />
      </div>
    </div>
  </section>
</template>

<script setup>
import { computed, ref, onMounted } from 'vue'
import { useMotion } from '@vueuse/motion'
import { projects } from '../data/projects.js'
import ProjectSection from './ProjectSection.vue'

const sectionRef = ref(null)

const visibleProjects = computed(() => {
  return projects.filter(project => project.visible === true)
})

const titleRef = ref(null)

onMounted(() => {
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
})
</script>

