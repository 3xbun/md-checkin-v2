<template>
  <div class="students-list">
    <strong>รายชื่อ</strong>
    <table>
      <tbody>
        <tr v-for="(student, index) in checkedLst">
          <td>{{ student.id }}</td>
          <td class="name">{{ student.name }}</td>
          <td class="time">{{ student.time }}</td>
          <td class="delete" @click="remove(index)"><ion-icon name="trash-outline"></ion-icon></td>
        </tr>
      </tbody>
    </table>
  </div>
  <div class="saveFile" @click="upload()"><ion-icon name="cloud-upload-outline"></ion-icon>
  </div>
</template>

<script setup>
import { inject } from 'vue';

const checkedLst = inject('checkedLst')

const remove = (index) => {
  checkedLst.value.splice(index, 1)
  sessionStorage.setItem('checkedLst', JSON.stringify(checkedLst.value))
}
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

td.name {
  width: 90%;
}

td.time {
  width: 10%;
}

.saveFile {
  position: absolute;
  bottom: 5vh;
  right: 5vh;
  background-color: #00B2CA;
  width: 1.5em;
  height: 1.5em;
  color: #F5F5F5;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  font-size: 30px;
}

tr {
  display: flex;
  align-items: center;
  gap: 1em;
}
</style>