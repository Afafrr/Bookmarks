<script setup lang="ts">
import { ref, onMounted } from 'vue';
import FolderTree from './components/FolderTree.vue';
import { data } from './data.js';

const bookmarks = ref<chrome.bookmarks.BookmarkTreeNode[]>([]);

onMounted(() => {
  if (typeof chrome !== 'undefined' && chrome.bookmarks) {
    chrome.bookmarks.getTree((nodes) => {
      bookmarks.value = nodes;
    });
  } else {
    console.warn('Chrome API not available (are you running in extension?)');
  }
});
</script>

<template>
  <div>Hello</div>
  <div>
    <div v-for="bookmark in bookmarks" :key="bookmark.id" type="single" collapsible>
      <pre>{{ bookmark }}</pre>
    </div>
  </div>
  <FolderTree :nodes="data.children" />
</template>

<style scoped></style>
