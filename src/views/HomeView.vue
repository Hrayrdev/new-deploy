<template>
  <div class="home">
    <img style="height: 50vh; width: 60vh"  alt="Vue logo" src="../assets/photo_2024-12-21_19-43-11.jpg">
    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App"/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import HelloWorld from '@/components/HelloWorld.vue'; // @ is an alias to /src

export default defineComponent({
  name: 'HomeView',
  components: {
    HelloWorld,
  },
});
</script>
