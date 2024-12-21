<template>
  <div class="home">

    <input v-model="inputValue" type="text">
    <button @click="validFunc">ОК</button>


    <div style="padding-top: 20px" v-if="lenor">
      <img style="height: 50vh; width: 60vh" alt="Vue logo" src="../assets/photo_2024-12-21_19-43-11.jpg">
    </div>
    <div style="padding-top: 20px" v-if="paracetamol">
      <img style="height: 50vh; width: 60vh" alt="Vue logo" src="../assets/paracetamol.jpg">
      <HelloWorld msg="Парацетамол"/>
    </div>

  </div>
</template>

<script setup lang="ts">
import {ref} from 'vue';
import HelloWorld from '@/components/HelloWorld.vue'; // @ is an alias to /src

const inputValue = ref('')
const lenor = ref(false)
const paracetamol = ref(false)

function validFunc() {
  if (inputValue.value === '999') {
    lenor.value = false
    paracetamol.value = true
  }
  if (inputValue.value === 'zqj') {
    paracetamol.value = false
    lenor.value = true
  }
}


</script>

<style scoped>
</style>