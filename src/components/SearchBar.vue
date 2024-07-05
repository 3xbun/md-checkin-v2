<template>
  <div class="search">
    <p class="searchBox">{{ searchID }}</p>
    <p class="error">{{ err }}</p>
    <div class="student" v-if="checkedLst[0]">
      <p v-if="student.name">{{ student.studentID }} | {{ student.name }}</p>
    </div>
    <ul class="numpad">
      <li @click="add(1)">1</li>
      <li @click="add(2)">2</li>
      <li @click="add(3)">3</li>
      <li @click="add(4)">4</li>
      <li @click="add(5)">5</li>
      <li @click="add(6)">6</li>
      <li @click="add(7)">7</li>
      <li @click="add(8)">8</li>
      <li @click="add(9)">9</li>
      <li @click="add(0)">0</li>
      <li @click="del()" class="delete">
        <ion-icon name="backspace-outline"></ion-icon>
      </li>
      <li @click="checkIn(searchID, 'id')" class="enter">
        <ion-icon name="enter-outline"></ion-icon>
      </li>
    </ul>
    <input class="cardInput" type="text" v-model="cardID" @keyup.enter="checkIn(cardID, 'cardID')"
      placeholder="Click here to scan card">
  </div>
</template>

<script setup>
import { inject, ref } from 'vue';

const searchID = ref('')
const cardID = ref('')
const student = ref({})
const err = ref('')

const checkedLst = inject('checkedLst')

const del = () => {
  searchID.value = searchID.value.replace(/.$/, '')
  err.value = ''
};

const add = (n) => {
  student.value = {}
  err.value = ''
  if (searchID.value.length <= 4) {
    searchID.value += n;
  }
}

const checkIn = (id, type) => {
  student.value = {}
  err.value = ''
  document.querySelector(".cardInput").focus()

  if (!id) {
    err.value = 'Student not found'
    searchID.value = ''
    cardID.value = ''
  }

  if (id.length < 5) {
    err.value = "Please Enter Student ID"
  } else {
    const date = new Date()
    if (checkedLst.value.filter(s => s.id === id).length > 0) {
      err.value = 'Already checked in'
      searchID.value = ''
      cardID.value = ''
      return
    }

    checkedLst.value.unshift({
      type: type,
      id: id,
      status: 0
    })

    searchID.value = ''
    cardID.value = ''
    sessionStorage.setItem('checkedLst', JSON.stringify(checkedLst.value))
  }
}
</script>

<style scoped>
.search {
  width: 60%;
  align-self: center;
}

.searchBox {
  font-size: 2em;
  text-align: center;
  letter-spacing: .5em;
  text-indent: -.5em;
  direction: rtl;
  height: 2em;
  border: 1px solid lightgray;
  border-radius: .5em;
  display: flex;
  justify-content: center;
  align-items: center;
}


.numpad {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: .5em;
  margin-top: 1em;
}

.numpad>li {
  list-style: none;
  width: 15%;
  text-align: center;
  border: 1px solid lightgrey;
  border-radius: 1em;
  height: 1.5em;
  display: flex;
  align-items: center;
  justify-content: center;
}

li:hover {
  cursor: pointer;
}

.enter {
  background-color: #00B2CA;
  border: 1px solid #00B2CA !important;
  color: #F5F5F5;
}

.error {
  color: lightcoral;
  text-align: center;
  margin-bottom: 1em;
}

.student {
  font-size: 30px;
  text-align: center;
}

.cardInput {
  border: none;
  width: 100%;
  text-align: center;
}

.cardInput:focus {
  outline: none;
}

.delete {
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: auto;
}
</style>