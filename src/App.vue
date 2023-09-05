<script setup>
import { ref, onMounted } from "vue";

import VuePagination from "../lib/VuePagination.vue";

const pages = ref(99);
const currentPage = ref(1);

function setPage(page) {
  currentPage.value = page
}
</script>

<template>
  <div>
    <vue-pagination :description="`Показано с 1 по ${50}`" :pages="pages" :current-page="currentPage" :limit="50" @set-page="setPage"></vue-pagination>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
