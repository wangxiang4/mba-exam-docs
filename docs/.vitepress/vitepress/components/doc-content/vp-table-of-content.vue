<script setup lang="ts">
import { ref } from 'vue'
import { useToc } from '../../composables/use-toc'
import { useActiveSidebarLinks } from '../../composables/active-bar'

const headers = useToc()
const container = ref()
useActiveSidebarLinks(container)
</script>

<template>

    <aside ref="container" class="toc-wrapper">
      <ElScrollbar class="toc-content">
        <nav>
          <h3 class="toc-content__heading">Contents</h3>
          <ul class="toc-items">
            <li v-for="{ link, text, children } in headers" :key="link" class="toc-item">
              <a class="toc-link" :href="link" :title="text">{{ text }}</a>
              <ul v-if="children">
                <li v-for="{ link: childLink, text: childText } in children" :key="childLink" class="toc-item">
                  <a class="toc-link subitem" :href="childLink" :title="text">
                    {{ childText }}
                  </a>
                </li>
              </ul>
            </li>
          </ul>
        </nav>
      </ElScrollbar>
    </aside>

</template>
