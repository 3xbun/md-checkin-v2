<template>
  <div class="students-list">
    <strong>รายชื่อ</strong>
    <table>
      <tbody>
        <Name v-for="student in checkedLst" :student="student" />
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { inject, computed } from 'vue';
import axios from 'axios';

const checkedLst = inject('checkedLst')
import Name from '../components/Name.vue';


</script>

<style scoped>
.students-list {
  width: 40%;
}

.students-list {
  margin-top: 6em;
  font-size: 20px;
  overflow: scroll;
}

.students-list>table {
  width: 100%;
}
</style>