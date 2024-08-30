<template>
  <div class="students-list">
    <p>
      <strong>รายชื่อ</strong>
      <span>
        มาเรียนแล้ว {{checkedLst.length}} คน
      </span>
    </p>
    <table>
      <tbody>
        <Name v-for="student in checkedLst" :student="student" />
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { inject } from 'vue';

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

span {
  font-size: .8em;
  color: lightgray
}
</style>