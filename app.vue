<template>
  <div class="container">
    <div>
      Preview
      <input v-model="preview" type="checkbox" />
    </div>
    <textarea
      v-show="!preview"
      v-model="body"
      placeholder="Write text..."
    ></textarea>
    <ContentRenderer v-if="preview" :value="previewAst" class="renderer">
      <template #empty>
        <div>
          <p>No text provided</p>
        </div>
      </template>
    </ContentRenderer>
  </div>
</template>

<script setup lang="ts">
import { transformContent } from '@nuxt/content/transformers';

const body = ref('');
const preview = ref(false);
const previewAst = ref(null);

watch([preview, body], async () => {
  if (preview.value) {
    previewAst.value = await transformContent(
      'content:_markdown.md',
      body.value || '',
      {}
    );
  }
});
</script>

<style scoped>
.container {
  border: solid black 1px;
  padding: 4px;
}
.renderer {
  min-height: 80px;
  background-color: rgba(80, 80, 80, 0.4);
}
</style>
