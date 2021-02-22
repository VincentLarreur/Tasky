<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <Todos/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Todos from '@/components/Todos.vue'; // @ is an alias to /src

@Component({
  components: {
    Todos,
  },
})
export default class Home extends Vue {}
</script>
