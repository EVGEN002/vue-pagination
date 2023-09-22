<script setup>
import { defineProps, ref, watchEffect, defineEmits } from 'vue'

import IChevronLeft from './IChevronLeft.vue'
import IChevronRight from './IChevronRight.vue'

const emit = defineEmits(['setPage'])

const start = ref(2)
const end = ref(4)
const firstNumber = ref(1)
const lastNumber = ref(5)

const props = defineProps({
  pages: Number,
  currentPage: Number,
  description: String
})

watchEffect(() => {
  if (props.currentPage > 4 && props.currentPage < props.pages) {
    start.value = props.currentPage - 2
    end.value = props.currentPage
    firstNumber.value = props.currentPage - 3
    lastNumber.value = props.currentPage + 1
  } else if (
    (props.currentPage === props.pages || props.currentPage > props.pages) &&
    props.currentPage > 4
  ) {
    start.value = props.currentPage - 3
    end.value = props.currentPage - 1
    firstNumber.value = props.currentPage - 4
    lastNumber.value = props.currentPage
  } else {
    start.value = 2
    end.value = 4
    firstNumber.value = 1
    lastNumber.value = 5
  }
})

function range(start, end) {
  if (start === end) {
    return [start]
  } else {
    return [start, ...range(start + 1, end)]
  }
}

function setPage(page) {
  emit('setPage', page)
  if (props.scroll === true) {
    window.scrollTo(0, 0)
  }
}

function toStart() {
  setPage(1)
  start.value = 2
  end.value = 4
  firstNumber.value = 1
  lastNumber.value = 5
}

function toEnd() {
  setPage(props.pages)
  start.value = props.pages - 3
  end.value = props.pages - 1
  firstNumber.value = props.pages - 4
  lastNumber.value = props.pages
}

function rightOffset(page) {
  if (page === props.pages) {
    setPage(props.pages)
  } else {
    setPage(page)
    start.value += 1
    end.value += 1
    firstNumber.value += 1
    lastNumber.value += 1
  }
}

function leftOffset(page) {
  if (page === 1) {
    setPage(1)
  } else {
    setPage(page)
    start.value -= 1
    end.value -= 1
    firstNumber.value -= 1
    lastNumber.value -= 1
  }
}

function offset(vector) {
  if (vector === 'right') {
    if (props.currentPage > end.value - 1) {
      rightOffset(props.currentPage + 1)
    } else {
      setPage(props.currentPage + 1)
    }
  } else if (vector === 'left') {
    if (props.currentPage < start.value + 1) {
      leftOffset(props.currentPage - 1)
    } else {
      setPage(props.currentPage - 1)
    }
  }
}
</script>

<template>
  <div class="vue-pagination">
    <p class="vue-pagination__description">{{ description }}</p>
    <div class="vue-pagination__pagination">
      <button
        :class="{ active: currentPage === 1 }"
        v-if="end > 4"
        @click="toStart()"
      >
        1
      </button>
      <button
        class="icon"
        :class="{ disabled: currentPage === 1 }"
        @click="offset('left')"
      >
        <i-chevron-left></i-chevron-left>
      </button>
      <button
        :class="[
          { disabled: firstNumber === currentPage },
          { active: currentPage === firstNumber }
        ]"
        @click="leftOffset(firstNumber)"
      >
        {{ firstNumber }}
      </button>
      <template v-for="page in range(start, end)">
        <button
          :class="[
            { active: currentPage === page },
            { disabled: page > pages }
          ]"
          v-if="page <= pages"
          @click="setPage(page)"
        >
          {{ page }}
        </button>
      </template>
      <button
        :class="[
          { disabled: lastNumber === currentPage },
          { active: currentPage === lastNumber }
        ]"
        v-if="lastNumber <= pages"
        @click="rightOffset(lastNumber)"
      >
        {{ lastNumber }}
      </button>
      <button
        class="icon"
        :class="{ disabled: currentPage >= pages }"
        @click="offset('right')"
      >
        <i-chevron-right></i-chevron-right>
      </button>
      <button v-if="pages > 5 && lastNumber !== pages" @click="toEnd()">
        {{ pages }}
      </button>
    </div>
  </div>
</template>

<style scoped lang="sass">
.vue-pagination
  display: flex
  align-items: center
  justify-content: space-between
  &__pagination
    display: flex
    align-items: center
button
  border: none
  background-color: transparent
  min-width: 2rem
  height: 2rem
  cursor: pointer
button.active
  border: none
  border-radius: .25em
  margin-left: .25rem
  margin-right: .25rem
  background-color: #0054a6
  color: #ffffff
button.icon
  border: none
  background: none
  display: flex
  align-items: center
  justify-content: center
  color: #313c52
button:not(.active):hover
  color: #0054a6
button.disabled
  color: #b1b4bb
  pointer-events: none
</style>
