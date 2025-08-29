<script setup lang="ts">
import { ref } from 'vue';
import ArrowIcon from '/src/components/assets/tree_arrow.svg';
import SmoothFolderFold from './FolderTree/SmoothFolderFold.vue';

defineProps<{
  nodes: chrome.bookmarks.BookmarkTreeNode[];
}>();

const openFolders = ref<Record<string, boolean>>({});

function toggleFolder(id: string) {
  openFolders.value[id] = !openFolders.value[id];
}
function getFavIcon(url: string | undefined) {
  return `https://www.google.com/s2/favicons?sz=32&domain=${url}`;
}
</script>
<!-- TODO: add icon for not found/undefined favicon -->
<template>
  <ul class="folder-tree">
    <li v-for="node in nodes" :key="node.id">
      <div class="item-container" @click="toggleFolder(node.id)">
        <ArrowIcon v-if="node.children" class="arrow-icon" />
        <img v-else class="fav-icons" :src="getFavIcon(node.url)" />
        <span :class="node.children ? 'folder-text' : 'link-text'">{{ node.title }}</span>
      </div>
      <SmoothFolderFold>
        <FolderTree v-if="node.children && openFolders[node.id]" :nodes="node.children" />
      </SmoothFolderFold>
    </li>
  </ul>
</template>

<style scoped>
.folder-tree {
  position: relative;
  width: 500px;
  user-select: none;
}
ul.folder-text {
  height: 30px;
}
ul.folder-tree,
ul.folder-tree li {
  list-style: none;
  padding-left: 15px;
}
ul.folder-tree li {
  width: 100%;
}
.folder-tree li > div:hover {
  background: #f0f4ff;
  cursor: pointer;
  border-radius: 4px;
}
.item-container {
  display: flex;
  align-items: center;
  height: 18px;
  padding: 2px 0px;
}
.item-container > span {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  display: inline-block;
  vertical-align: middle;
}

.arrow-icon {
  fill: black;
  width: 20px;
  height: auto;
}
.fav-icons {
  width: 16px;
  height: auto;
  padding-right: 3px;
}
.folder-text {
  font-weight: 500;
}
</style>
