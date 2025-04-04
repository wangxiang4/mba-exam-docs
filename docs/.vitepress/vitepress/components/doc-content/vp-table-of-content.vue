<script setup lang="ts">
import { ref } from 'vue'
import { useToc } from '~/composables/use-toc'
import { useActiveSidebarLinks } from '~/composables/active-bar'
import { renderTeX, clearRenderSettingsRegex } from '~/utils/tex2svg'

const headers = useToc()
const container = ref()
useActiveSidebarLinks(container)


function renderTableOfContents(content: string): string {
  let renderedContent: string
  // Trim the excess to remove unnecessary characters
  let trimmedContent = content.trim()
                              .replace(clearRenderSettingsRegex, '')

  const mathInlineRegex = /(?<!\\|\$)\$(?!\$|\s|\t)((?:[^$]|\\\$)*)(?<!\\|\s|\t)\$(?!\$)/g

  // Rendering Mathematical Formulas
  renderedContent = trimmedContent.replace(mathInlineRegex, (match, formula) =>
    renderTeX(formula, { rset: { wdiv: false }}))

  return renderedContent
}

</script>

<template>

    <aside ref="container" class="toc-wrapper">
      <ElScrollbar class="toc-content">
        <nav>
          <h3 class="toc-content__heading">Contents</h3>
          <ul class="toc-items">
            <li v-for="{ link, text, children } in headers" :key="link" class="toc-item">
              <a class="toc-link" :href="link" :title="text">
                <span v-html="renderTableOfContents(text)" />
              </a>
              <ul v-if="children">
                <li v-for="{ link: childLink, text: childText } in children" :key="childLink" class="toc-item">
                  <a class="toc-link subitem" :href="childLink" :title="text">
                    <span v-html="renderTableOfContents(childText)" />
                  </a>
                </li>
              </ul>
            </li>
          </ul>
        </nav>
      </ElScrollbar>
    </aside>

</template>
