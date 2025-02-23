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
    <div style="padding-top: 20px" v-if="elen">
      <HelloWorld msg="Тут должна быть твоя фотография Элен 💔"/>
    </div>

    <casino v-if="benik"/>

  </div>
</template>

<script setup lang="ts">
import {ref} from 'vue';
import HelloWorld from '@/components/HelloWorld.vue';
import Casino from "@/components/casino.vue";

const inputValue = ref('')
const lenor = ref(false)
const paracetamol = ref(false)
const benik = ref(false)
const elen = ref(false)

function validFunc() {
  if (inputValue.value === '999') {
    lenor.value = false
    elen.value = false
    benik.value=false
    paracetamol.value = true
  }
  if (inputValue.value === 'zqj') {
    paracetamol.value = false
    elen.value = false
    lenor.value = true
    benik.value=false

  }
  if (inputValue.value === 'Беник') {
    paracetamol.value = false
    elen.value = false
    lenor.value = false
    benik.value=true
  }
  if (inputValue.value === 'Любовь всей моей жизни') {
    paracetamol.value = false
    lenor.value = false
    benik.value=false
    elen.value = true
  }
}


</script>

<style scoped>
.vanya {
  margin-top: 20px;
  gap: 5px;
  display: grid;
  grid-template-columns: 1fr ;
}

.vanya-img-wrapper {
  height: 350px;
  display: flex;
  justify-content: center;
}
.vanya-img {
  display: flex;

}

</style>