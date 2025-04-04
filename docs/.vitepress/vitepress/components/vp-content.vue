<script setup lang="ts">
import { computed, nextTick, onUpdated, ref, watch } from 'vue'
import nprogress from 'nprogress'
import { useData, useRoute } from 'vitepress'
import { useSidebar } from '../composables/sidebar'
import VPHeroContent from './vp-hero-content.vue'
import VPDocContent from './vp-doc-content.vue'
import VPNotFound from './vp-not-found.vue'

const { frontmatter } = useData()
const route = useRoute()
const isNotFound = computed(() => route.component === VPNotFound)
const isHeroPost = computed(() => frontmatter.value.page === true)
const { hasSidebar } = useSidebar()

const props = defineProps<{ isSidebarOpen: boolean }>()

const shouldUpdateProgress = ref(true)

watch(
    () => props.isSidebarOpen,
    (val) => {
      // delay the flag update since watch is called before onUpdated
      nextTick(() => {
        shouldUpdateProgress.value = !val
      })
    }
)

onUpdated(() => {
  // Safari:solve init order wrong,lead to onUpdated then clickEventListener execute first
  // Delay updating navigation bar only then will VPDocContent content be cached after click
  setTimeout(() => {
    if (shouldUpdateProgress.value) {
      nprogress.done()
    }
  }, 100)
})
</script>

<template>
  <main :class="{ 'page-content': true, 'has-sidebar': hasSidebar }">
    <VPNotFound v-if="isNotFound" />
    <VPHeroContent v-else-if="isHeroPost" />
    <VPDocContent v-else/>
  </main>
</template>
