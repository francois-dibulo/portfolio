<template>
  <section
    ref="sectionRef"
    class="relative py-32 flex flex-col w-full m-auto bg-base-100 text-white overflow-hidden"
    aria-label="Projects"
  >

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
    // Apply initial state immediately
    useMotion(titleRef.value, {
      initial: { opacity: 0, y: 30 }
    })
    
    // Trigger enter animation after a brief delay to ensure initial state is applied
    requestAnimationFrame(() => {
      useMotion(titleRef.value, {
        enter: { 
          opacity: 1, 
          y: 0, 
          transition: { duration: 800, ease: [0.16, 1, 0.3, 1] }
        }
      })
    })
  }
})
</script>

