<template>
  <div class="container">
    <SearchBar />
    <CheckedLst />
  </div>
</template>

<script setup>
import { onMounted, provide, ref } from 'vue';
import SearchBar from './components/SearchBar.vue';
import CheckedLst from './components/CheckedLst.vue';

const checkedLst = ref([])

provide("checkedLst", checkedLst)

onMounted(() => {
  document.querySelector(".cardInput").focus()
  if (sessionStorage.getItem('checkedLst')) {
    checkedLst.value = JSON.parse(sessionStorage.getItem('checkedLst'))
  }
})
</script>

<style scoped></style>