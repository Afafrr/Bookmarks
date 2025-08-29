<script setup lang="ts">
function beforeEnter(el: Element) {
  if (!(el instanceof HTMLElement)) return;
  el.style.maxHeight = '0';
  el.style.opacity = '0';
}

function enter(el: Element, done: () => void) {
  if (!(el instanceof HTMLElement)) return;
  el.style.transition = 'max-height 0.3s cubic-bezier(0.4,0,0.2,1), opacity 0.35s';
  el.style.maxHeight = el.scrollHeight + 'px';
  el.style.opacity = '1';
  const cleanup = () => {
    el.style.maxHeight = 'none';
    el.removeEventListener('transitionend', cleanup);
    done();
  };
  el.addEventListener('transitionend', cleanup);
}

function beforeLeave(el: Element) {
  if (!(el instanceof HTMLElement)) return;
  el.style.maxHeight = el.scrollHeight + 'px';
}

function leave(el: Element, done: () => void) {
  if (!(el instanceof HTMLElement)) return;
  el.style.transition = 'max-height 0.25s cubic-bezier(0.4,0,0.2,1), opacity 0.3s';
  el.style.maxHeight = '0';
  el.style.opacity = '0';
  const cleanup = () => {
    el.style.maxHeight = 'none';
    el.removeEventListener('transitionend', cleanup);
    done();
  };
  el.addEventListener('transitionend', cleanup);
}
</script>

<template>
  <Transition name="fold" @before-enter="beforeEnter" @enter="enter" @before-leave="beforeLeave" @leave="leave">
    <slot />
  </Transition>
</template>

<style scoped>
.fold-enter-active,
.fold-leave-active {
  overflow: hidden;
}
</style>
