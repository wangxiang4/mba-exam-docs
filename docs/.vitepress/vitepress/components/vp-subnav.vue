<script setup lang="ts">
import { useSidebar } from '../composables/sidebar'
import { useBackTop } from '../composables/back-top'
import ToggleSidebarBtn from './subnav/toggle-sidebar-btn.vue'
import { useData } from 'vitepress';
import homeLocale from "../../i18n/pages/home.json"
defineEmits(['open-menu'])

const { hasSidebar } = useSidebar()
const { shouldShow, scrollToTop } = useBackTop()
const { lang } = useData(), locale = homeLocale[lang.value]
</script>

<template>
  <div class="sub-nav py-3 flex items-center">
    <ToggleSidebarBtn v-if="hasSidebar" @click="$emit('open-menu')" />
    <Transition name="shifting">
      <ElLink :class="{ 'go-back-top': true, show: shouldShow }" :underline="false" class="height-5" size="small" @click.prevent.stop="scrollToTop">
        {{ locale['back-top'] }}
      </ElLink>
    </Transition>
  </div>
</template>
