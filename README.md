## 🔎 Preview

Simple Vue pagination component

<img src="https://ibb.co/FBWjMmD"></img>

## ⚙ Install

```
npm install @evgen002/vue-pagination
```

## 📄 Example
```
<script setup>
import { ref } from 'vue'

import VuePagination from '@evgen002/vue-pagination'

const pages = ref(99)
const currentPage = ref(1)

function setPage(page) {
  currentPage.value = page
}
</script>

<template>
  <div>
    <vue-pagination
      :description="`Показано с 1 по ${50}`"
      :pages="pages"
      :current-page="currentPage"
      :limit="50"
      @set-page="setPage"
    ></vue-pagination>
  </div>
</template>
```