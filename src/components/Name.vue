<template>
  <tr>
    <td v-if="student.status == 0">{{ student.id }}</td>
    <!-- <td v-if="student.status == 1">{{ student.checkin }}</td> -->
    <td v-if="student.status == 1">
      {{ dayjs(lastCheckIn).format('HH:mm') }}
    </td>
    <td>{{ student.stdID }}</td>
    <td class="name">{{ student.name }}</td>
    <td>
      <div v-if="student.status == 0" class="loader"></div>
    </td>
    <td @click="remove(student)">x</td>
  </tr>
</template>

<script setup>
import { ref, onMounted, onUpdated, inject, toRefs, computed } from 'vue';
import axios from 'axios';
import dayjs from 'dayjs';

const props = defineProps({
  student: Object
})

const { student } = toRefs(props)

const lastCheckIn = ref('')

const checkedLst = inject('checkedLst')

const remove = (student) => {
  checkedLst.value = checkedLst.value.filter(std => {
    console.log(std);
    return std != student
  })
  sessionStorage.setItem('checkedLst', JSON.stringify(checkedLst.value))
}

const getName = async () => {
  const std = props.student
  const payload = {
    "type": std.type,
    "id": std.id
  }

  if (std.status == 0) {
    const student = await axios.post("https://md-regis-api.3xbun.com/checkin", payload).then(res => {
      return res.data.data
    }).catch(err => {
      std.status = 2
      console.log(std);
      console.log(err);
    })

    if (student) {
      std.name = student.fullNameTH
      std.status = 1
      std.stdID = student.stdID
      std.checkin = student
    }
  } else {
    axios.get('https://md-regis-api.3xbun.com/Users/'+student.value.checkin.username).then(res => lastCheckIn.value = res.data.checkIns.at(-1))
  }

  sessionStorage.setItem('checkedLst', JSON.stringify(checkedLst.value))
}

onUpdated(() => {
  getName()
})

onMounted(() => {
  getName()
})

</script>

<style scoped>
td.name {
  width: 90%;
}

td.time {
  width: 10%;
}

tr {
  display: flex;
  align-items: center;
  gap: 1em;
}

/* HTML: <div class="loader"></div> */
.loader {
  width: 1.5em;
  aspect-ratio: 1;
  display: flex;
  color: #000;
  border: 2px solid;
  box-sizing: border-box;
  border-radius: 50%;
  background:
    radial-gradient(circle 2px, currentColor 95%, #0000),
    linear-gradient(currentColor 50%, #0000 0) 50%/2px 60% no-repeat;
  animation: l1 2s infinite linear;
}

.loader:before {
  content: "";
  flex: 1;
  background: linear-gradient(currentColor 50%, #0000 0) 50%/2px 80% no-repeat;
  animation: inherit;
}

@keyframes l1 {
  100% {
    transform: rotate(1turn)
  }
}
</style>
