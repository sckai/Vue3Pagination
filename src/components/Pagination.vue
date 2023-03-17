<script setup lang="ts">
import { computed } from 'vue'

// props
const props = defineProps({
  currentPage: {
    type: Number,
    default: 1
  },
  total: {
    type: Number,
    default: 1
  },
  middlePageDisplay: {
    type: Number,
    default: 3
  }
})

// emit
const emit = defineEmits(['GetPage'])
const GetPage = (page: number) => {
  emit("GetPage", page)
}

// computed
const showOneAfterManyButton = computed(() => {
  if ((props.total > props.middlePageDisplay + 2) && props.currentPage > props.middlePageDisplay) return true
  return false
})

const showLastAfterManyButton = computed(() => {
  if ((props.total > props.middlePageDisplay + 2) && props.total - (props.middlePageDisplay - 1) > props.currentPage) return true
  return false
})

const shwoLastNumButton = computed(() => {
  if (props.total > (props.middlePageDisplay + 1)) return true
  return false
})

const showMiddleButton = computed(() => {
  if (props.total > 1) return true
  return false
})

const middleNums = computed(() => {
  let nums = []
  if (props.total < (props.middlePageDisplay + 2)) {
    const displayNums = props.total - 1
    for (let index = 0; index < displayNums; index++) {
      nums[index] = index + 2
    }
    return nums
  }

  if (props.currentPage < (props.middlePageDisplay + 1)) {
    for (let index = 1; index < (props.middlePageDisplay + 1); index++) {
      nums.push(index + 1)
    }
    return nums
  }

  if (props.currentPage >= (props.middlePageDisplay + 1) && props.total - props.middlePageDisplay >= props.currentPage) {
    for (let index = 1; index <= props.middlePageDisplay; index++) {
      if (index === 1) {
        nums.push(props.currentPage)
        continue
      }
      if (index % 2 === 0) {
        nums.push(props.currentPage + (index / 2))
        continue
      }
      nums.unshift( Math.ceil(props.currentPage - (index / 2)))
    }
    return nums
  }

  for (let index = 1; index <= props.middlePageDisplay; index++) {
    nums.unshift(props.total - index)
  }
  return nums
})
const showPreviousButton = computed(() => {
  if (props.currentPage === 1) return false
  return true
})

const showNextButton = computed(() => {
  if (props.total === props.currentPage) return false
  return true
})

// event
const ClickPage = (page: number) => {
  GetPage(page)
}
</script>

<template lang="pug">
#Pagination
  .container
    .page(v-show="showPreviousButton"
      @click="ClickPage(currentPage - 1)")
      svg(aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg")
        path(fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd")

    .page(@click="ClickPage(1)"
      :class="{'currentPage': currentPage === 1}")
      span {{ `1` }}

    .page(v-show="showOneAfterManyButton")
      span {{ `...` }}

    .page(v-show="showMiddleButton"
      v-for="(num, index) in middleNums"
      :key="index"
      :class="{'currentPage': currentPage === num}"
      @click="ClickPage(num)")
      span  {{ `${num}` }}

    .page(v-show="showLastAfterManyButton")
      span {{ `...` }}

    .page(v-show="shwoLastNumButton"
      :class="{'currentPage': currentPage === total}"
      @click="ClickPage(total)")
      span {{ `${total}` }}

    .page(v-show="showNextButton"
      @click="ClickPage(currentPage + 1)")
      svg(aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg")
        path(fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd")
</template>

<style lang="scss" scoped>
#Pagination {
  & .container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    list-style: none;
    gap: 16px;
    & .page {
      display: flex;
      justify-content: center;
      align-items: center;
      min-width: 32px;
      height: 26px;
      color: #6b7280;
      padding: 6px 4px;
      border-color: #6b7280;
      border: 1px solid #6b7280;
      border-radius: 4px;
      cursor: pointer;
      user-select: none;
    }
    & .page:hover, .currentPage {
      background-color: #9ab4db;
      border-color: #6b7280;
      color: #ffffff;
    }
  }
}
</style>